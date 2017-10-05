---
title: "Översikt över aktören-baserad Azure mikrotjänster livscykel | Microsoft Docs"
description: "Beskriver Service Fabric tillförlitliga aktören livscykel, skräpinsamling och manuellt ta bort aktörer och deras tillstånd"
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: 75b7b77a0bef2051599a4f61183109cfb2ffff3b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a><span data-ttu-id="b1baf-103">Aktören livscykel, automatisk skräpinsamling och manuellt ta bort</span><span class="sxs-lookup"><span data-stu-id="b1baf-103">Actor lifecycle, automatic garbage collection, and manual delete</span></span>
<span data-ttu-id="b1baf-104">En aktör aktiveras första gången ett anrop görs till någon av dess metoder.</span><span class="sxs-lookup"><span data-stu-id="b1baf-104">An actor is activated the first time a call is made to any of its methods.</span></span> <span data-ttu-id="b1baf-105">En aktör är inaktiverad (skräp som samlats in av aktörer runtime) om den inte används för en konfigurerbar tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="b1baf-105">An actor is deactivated (garbage collected by the Actors runtime) if it is not used for a configurable period of time.</span></span> <span data-ttu-id="b1baf-106">En aktör och dess tillstånd kan också tas bort manuellt när som helst.</span><span class="sxs-lookup"><span data-stu-id="b1baf-106">An actor and its state can also be deleted manually at any time.</span></span>

## <a name="actor-activation"></a><span data-ttu-id="b1baf-107">Aktören aktivering</span><span class="sxs-lookup"><span data-stu-id="b1baf-107">Actor activation</span></span>
<span data-ttu-id="b1baf-108">När en aktör aktiveras inträffar följande:</span><span class="sxs-lookup"><span data-stu-id="b1baf-108">When an actor is activated, the following occurs:</span></span>

* <span data-ttu-id="b1baf-109">När ett samtal kommer för en aktör och ingen sådan redan är aktiv, skapas en ny aktören.</span><span class="sxs-lookup"><span data-stu-id="b1baf-109">When a call comes for an actor and one is not already active, a new actor is created.</span></span>
* <span data-ttu-id="b1baf-110">Den aktörstillstånd har lästs in om den underhåller tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b1baf-110">The actor's state is loaded if it's maintaining state.</span></span>
* <span data-ttu-id="b1baf-111">Den `OnActivateAsync` (C#) eller `onActivateAsync` (Java)-metoden (som kan åsidosättas i aktören implementeringen) anropas.</span><span class="sxs-lookup"><span data-stu-id="b1baf-111">The `OnActivateAsync` (C#) or `onActivateAsync` (Java) method (which can be overridden in the actor implementation) is called.</span></span>
* <span data-ttu-id="b1baf-112">Aktören anses nu aktiv.</span><span class="sxs-lookup"><span data-stu-id="b1baf-112">The actor is now considered active.</span></span>

## <a name="actor-deactivation"></a><span data-ttu-id="b1baf-113">Aktören avaktivering</span><span class="sxs-lookup"><span data-stu-id="b1baf-113">Actor deactivation</span></span>
<span data-ttu-id="b1baf-114">När en aktör inaktiveras inträffar följande:</span><span class="sxs-lookup"><span data-stu-id="b1baf-114">When an actor is deactivated, the following occurs:</span></span>

* <span data-ttu-id="b1baf-115">När en aktör inte används för vissa tidsperiod, bort den från tabellen Active aktörer.</span><span class="sxs-lookup"><span data-stu-id="b1baf-115">When an actor is not used for some period of time, it is removed from the Active Actors table.</span></span>
* <span data-ttu-id="b1baf-116">Den `OnDeactivateAsync` (C#) eller `onDeactivateAsync` (Java)-metoden (som kan åsidosättas i aktören implementeringen) anropas.</span><span class="sxs-lookup"><span data-stu-id="b1baf-116">The `OnDeactivateAsync` (C#) or `onDeactivateAsync` (Java) method (which can be overridden in the actor implementation) is called.</span></span> <span data-ttu-id="b1baf-117">Rensar alla timers för aktören.</span><span class="sxs-lookup"><span data-stu-id="b1baf-117">This clears all the timers for the actor.</span></span> <span data-ttu-id="b1baf-118">Aktören åtgärder som tillstånd ändringar inte ska anropas från den här metoden.</span><span class="sxs-lookup"><span data-stu-id="b1baf-118">Actor operations like state changes should not be called from this method.</span></span>

> [!TIP]
> <span data-ttu-id="b1baf-119">Fabric aktörer runtime skickar vissa [händelser relaterade till aktören aktivering och inaktivering av](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="b1baf-119">The Fabric Actors runtime emits some [events related to actor activation and deactivation](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span></span> <span data-ttu-id="b1baf-120">De är användbara i diagnostik- och prestandaövervakning.</span><span class="sxs-lookup"><span data-stu-id="b1baf-120">They are useful in diagnostics and performance monitoring.</span></span>
>
>

### <a name="actor-garbage-collection"></a><span data-ttu-id="b1baf-121">Aktören skräpinsamling</span><span class="sxs-lookup"><span data-stu-id="b1baf-121">Actor garbage collection</span></span>
<span data-ttu-id="b1baf-122">När en aktör inaktiveras referenser till objektet aktören släpps och det kan vara skräpinsamlats normalt av CLR (CLR) eller java virtual machine (JVM) skräpinsamlingen.</span><span class="sxs-lookup"><span data-stu-id="b1baf-122">When an actor is deactivated, references to the actor object are released and it can be garbage collected normally by the common language runtime (CLR) or java virtual machine (JVM) garbage collector.</span></span> <span data-ttu-id="b1baf-123">Skräpinsamling endast rensar aktören-objektet. Det gör **inte** ta bort statusen som lagras i aktören tillstånd Manager.</span><span class="sxs-lookup"><span data-stu-id="b1baf-123">Garbage collection only cleans up the actor object; it does **not** remove state stored in the actor's State Manager.</span></span> <span data-ttu-id="b1baf-124">Nästa gång aktören aktiveras aktören objekt skapas och dess tillstånd har återställts.</span><span class="sxs-lookup"><span data-stu-id="b1baf-124">The next time the actor is activated, a new actor object is created and its state is restored.</span></span>

<span data-ttu-id="b1baf-125">Vad räknas som ”används” för inaktivering och skräpinsamling?</span><span class="sxs-lookup"><span data-stu-id="b1baf-125">What counts as “being used” for the purpose of deactivation and garbage collection?</span></span>

* <span data-ttu-id="b1baf-126">Ta emot ett samtal</span><span class="sxs-lookup"><span data-stu-id="b1baf-126">Receiving a call</span></span>
* <span data-ttu-id="b1baf-127">`IRemindable.ReceiveReminderAsync`metoden anropas (gäller endast om aktören använder påminnelser)</span><span class="sxs-lookup"><span data-stu-id="b1baf-127">`IRemindable.ReceiveReminderAsync` method being invoked (applicable only if the actor uses reminders)</span></span>

> [!NOTE]
> <span data-ttu-id="b1baf-128">om aktören använder timers och dess timer-återanropet anropas, sker **inte** antal som ”används”.</span><span class="sxs-lookup"><span data-stu-id="b1baf-128">if the actor uses timers and its timer callback is invoked, it does **not** count as "being used".</span></span>
>
>

<span data-ttu-id="b1baf-129">Innan vi gå in information om inaktiveringen är det viktigt att definiera följande villkor:</span><span class="sxs-lookup"><span data-stu-id="b1baf-129">Before we go into the details of deactivation, it is important to define the following terms:</span></span>

* <span data-ttu-id="b1baf-130">*Skanna intervall*.</span><span class="sxs-lookup"><span data-stu-id="b1baf-130">*Scan interval*.</span></span> <span data-ttu-id="b1baf-131">Detta är det intervall då aktörer runtime söker igenom Active aktörer tabellen aktörer kan inaktiveras och skräpinsamlats.</span><span class="sxs-lookup"><span data-stu-id="b1baf-131">This is the interval at which the Actors runtime scans its Active Actors table for actors that can be deactivated and garbage collected.</span></span> <span data-ttu-id="b1baf-132">Standardvärdet för det här är 1 minut.</span><span class="sxs-lookup"><span data-stu-id="b1baf-132">The default value for this is 1 minute.</span></span>
* <span data-ttu-id="b1baf-133">*Inaktivitetstid*.</span><span class="sxs-lookup"><span data-stu-id="b1baf-133">*Idle timeout*.</span></span> <span data-ttu-id="b1baf-134">Det här är tidsperiod som en aktör behöver förblir oanvänt (inaktiv) innan den kan inaktiveras och skräpinsamlats.</span><span class="sxs-lookup"><span data-stu-id="b1baf-134">This is the amount of time that an actor needs to remain unused (idle) before it can be deactivated and garbage collected.</span></span> <span data-ttu-id="b1baf-135">Standardvärdet för det här är 60 minuter.</span><span class="sxs-lookup"><span data-stu-id="b1baf-135">The default value for this is 60 minutes.</span></span>

<span data-ttu-id="b1baf-136">Vanligtvis behöver du inte ändra standardinställningarna.</span><span class="sxs-lookup"><span data-stu-id="b1baf-136">Typically, you do not need to change these defaults.</span></span> <span data-ttu-id="b1baf-137">Men om det behövs dessa intervall kan ändras via `ActorServiceSettings` när du registrerar din [aktören Service](service-fabric-reliable-actors-platform.md):</span><span class="sxs-lookup"><span data-stu-id="b1baf-137">However, if necessary, these intervals can be changed through `ActorServiceSettings` when registering your [Actor Service](service-fabric-reliable-actors-platform.md):</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
    }
}
```
<span data-ttu-id="b1baf-138">Aktören runtime håller reda på hur lång tid som den har varit inaktiv (dvs. inte används) för varje active aktören.</span><span class="sxs-lookup"><span data-stu-id="b1baf-138">For each active actor, the actor runtime keeps track of the amount of time that it has been idle (i.e. not used).</span></span> <span data-ttu-id="b1baf-139">Aktören runtime kontrollerar var och en av berörda varje `ScanIntervalInSeconds` att se om det kan vara skräp samlas in och samlar in om det har varit inaktiv i `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="b1baf-139">The actor runtime checks each of the actors every `ScanIntervalInSeconds` to see if it can be garbage collected and collects it if it has been idle for `IdleTimeoutInSeconds`.</span></span>

<span data-ttu-id="b1baf-140">Varje gång som en aktör används återställs dess inaktivitetstid till 0.</span><span class="sxs-lookup"><span data-stu-id="b1baf-140">Anytime an actor is used, its idle time is reset to 0.</span></span> <span data-ttu-id="b1baf-141">Sedan kan aktören kan vara skräpinsamlats endast om den igen är vilande för `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="b1baf-141">After this, the actor can be garbage collected only if it again remains idle for `IdleTimeoutInSeconds`.</span></span> <span data-ttu-id="b1baf-142">Kom ihåg att en aktör betraktas som används om en gränssnittsmetod aktören eller ett aktören påminnelse motanrop körs.</span><span class="sxs-lookup"><span data-stu-id="b1baf-142">Recall that an actor is considered to have been used if either an actor interface method or an actor reminder callback is executed.</span></span> <span data-ttu-id="b1baf-143">En aktör är **inte** betraktas som används om dess timer motanrop körs.</span><span class="sxs-lookup"><span data-stu-id="b1baf-143">An actor is **not** considered to have been used if its timer callback is executed.</span></span>

<span data-ttu-id="b1baf-144">Följande diagram visar livscykeln för ett enda aktören att illustrera dessa begrepp.</span><span class="sxs-lookup"><span data-stu-id="b1baf-144">The following diagram shows the lifecycle of a single actor to illustrate these concepts.</span></span>

![Exempel på inaktivitetstid][1]

<span data-ttu-id="b1baf-146">Exemplet visar effekten av aktören metodanrop, påminnelser och timers på den här aktören livstid.</span><span class="sxs-lookup"><span data-stu-id="b1baf-146">The example shows the impact of actor method calls, reminders, and timers on the lifetime of this actor.</span></span> <span data-ttu-id="b1baf-147">Följande punkter om exemplet är värt att nämna:</span><span class="sxs-lookup"><span data-stu-id="b1baf-147">The following points about the example are worth mentioning:</span></span>

* <span data-ttu-id="b1baf-148">ScanInterval och IdleTimeout ställs till 5 och 10.</span><span class="sxs-lookup"><span data-stu-id="b1baf-148">ScanInterval and IdleTimeout are set to 5 and 10 respectively.</span></span> <span data-ttu-id="b1baf-149">(Enheter inte roll här, eftersom exemplet är bara för att illustrera begreppen.)</span><span class="sxs-lookup"><span data-stu-id="b1baf-149">(Units do not matter here, since our purpose is only to illustrate the concept.)</span></span>
* <span data-ttu-id="b1baf-150">Genomsökningen efter aktörer på att skräpinsamlas sker på T = 0, 5, 10, 15, 20, 25, som definieras i intervallet för sökning på 5.</span><span class="sxs-lookup"><span data-stu-id="b1baf-150">The scan for actors to be garbage collected happens at T=0,5,10,15,20,25, as defined by the scan interval of 5.</span></span>
* <span data-ttu-id="b1baf-151">En periodisk timer utlöses vid T = 4, 8, 12, 16, 20, 24, och dess motanrop körs.</span><span class="sxs-lookup"><span data-stu-id="b1baf-151">A periodic timer fires at T=4,8,12,16,20,24, and its callback executes.</span></span> <span data-ttu-id="b1baf-152">Inaktivitetstid aktören påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="b1baf-152">It does not impact the idle time of the actor.</span></span>
* <span data-ttu-id="b1baf-153">En aktören metodanrop på T = 7 återställs den inaktiva tiden till 0 och försenar skräpinsamling aktören.</span><span class="sxs-lookup"><span data-stu-id="b1baf-153">An actor method call at T=7 resets the idle time to 0 and delays the garbage collection of the actor.</span></span>
* <span data-ttu-id="b1baf-154">Ett återanrop för aktören påminnelse körs på T = 14 och ytterligare fördröjningar skräpinsamling aktören.</span><span class="sxs-lookup"><span data-stu-id="b1baf-154">An actor reminder callback executes at T=14 and further delays the garbage collection of the actor.</span></span>
* <span data-ttu-id="b1baf-155">Under skräp samling avsökningen T = 25 skådespelare, inaktivitetstid slutligen överskrider tidsgränsen för inaktivitet 10 och aktören samlas in som skräp.</span><span class="sxs-lookup"><span data-stu-id="b1baf-155">During the garbage collection scan at T=25, the actor's idle time finally exceeds the idle timeout of 10, and the actor is garbage collected.</span></span>

<span data-ttu-id="b1baf-156">En aktör blir aldrig skräpinsamlats medan det körs en av dess metoder, oavsett hur lång tid det tar vid körning av den här metoden.</span><span class="sxs-lookup"><span data-stu-id="b1baf-156">An actor will never be garbage collected while it is executing one of its methods, no matter how much time is spent in executing that method.</span></span> <span data-ttu-id="b1baf-157">Som tidigare nämnts förhindrar körning av aktören gränssnittsmetoder och påminnelse återanrop skräpinsamling genom att återställa den aktören inaktivitetstid till 0.</span><span class="sxs-lookup"><span data-stu-id="b1baf-157">As mentioned earlier, the execution of actor interface methods and reminder callbacks prevents garbage collection by resetting the actor's idle time to 0.</span></span> <span data-ttu-id="b1baf-158">Körningen av timer återanrop återställs inte den inaktiva tiden till 0.</span><span class="sxs-lookup"><span data-stu-id="b1baf-158">The execution of timer callbacks does not reset the idle time to 0.</span></span> <span data-ttu-id="b1baf-159">Skräpinsamling aktören skjuts tills timer-återanrop har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b1baf-159">However, the garbage collection of the actor is deferred until the timer callback has completed execution.</span></span>

## <a name="deleting-actors-and-their-state"></a><span data-ttu-id="b1baf-160">Ta bort aktörer och deras tillstånd</span><span class="sxs-lookup"><span data-stu-id="b1baf-160">Deleting actors and their state</span></span>
<span data-ttu-id="b1baf-161">Skräpinsamling inaktiverade aktörer endast rensar aktören objektet, men det tar inte bort data som lagras i en aktör tillstånd Manager.</span><span class="sxs-lookup"><span data-stu-id="b1baf-161">Garbage collection of deactivated actors only cleans up the actor object, but it does not remove data that is stored in an actor's State Manager.</span></span> <span data-ttu-id="b1baf-162">När en aktör aktiveras igen, få dess data igen ska tillgång till den via hanteraren tillstånd.</span><span class="sxs-lookup"><span data-stu-id="b1baf-162">When an actor is re-activated, its data is again made available to it through the State Manager.</span></span> <span data-ttu-id="b1baf-163">I fall där aktörer lagra data i Tillståndshanterare och är inaktiverad men aldrig aktiverats igen, kan det vara nödvändigt att rensa sina data.</span><span class="sxs-lookup"><span data-stu-id="b1baf-163">In cases where actors store data in State Manager and are deactivated but never re-activated, it may be necessary to clean up their data.</span></span>

<span data-ttu-id="b1baf-164">Den [aktören Service](service-fabric-reliable-actors-platform.md) innehåller en funktion för att ta bort aktörer från en fjärransluten anropare:</span><span class="sxs-lookup"><span data-stu-id="b1baf-164">The [Actor Service](service-fabric-reliable-actors-platform.md) provides a function for deleting actors from a remote caller:</span></span>

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

<span data-ttu-id="b1baf-165">Om du tar bort en aktör ger följande effekter beroende på om huruvida aktören är för närvarande är aktiva:</span><span class="sxs-lookup"><span data-stu-id="b1baf-165">Deleting an actor has the following effects depending on whether or not the actor is currently active:</span></span>

* <span data-ttu-id="b1baf-166">**Aktiva aktören**</span><span class="sxs-lookup"><span data-stu-id="b1baf-166">**Active Actor**</span></span>
  * <span data-ttu-id="b1baf-167">Aktören tas bort från listan med aktiva aktörer och inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="b1baf-167">Actor is removed from active actors list and is deactivated.</span></span>
  * <span data-ttu-id="b1baf-168">Dess tillstånd tas bort permanent.</span><span class="sxs-lookup"><span data-stu-id="b1baf-168">Its state is deleted permanently.</span></span>
* <span data-ttu-id="b1baf-169">**Inaktiva aktören**</span><span class="sxs-lookup"><span data-stu-id="b1baf-169">**Inactive Actor**</span></span>
  * <span data-ttu-id="b1baf-170">Dess tillstånd tas bort permanent.</span><span class="sxs-lookup"><span data-stu-id="b1baf-170">Its state is deleted permanently.</span></span>

<span data-ttu-id="b1baf-171">Observera att det går inte att anropa en aktör ta bort på sig själv från någon av dess metoder aktören eftersom aktören inte kan tas bort körs inom en aktören anrop-kontext där körningsmiljön har fått ett lås runt aktören anropet att tillämpa Enkeltrådig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b1baf-171">Note that an actor cannot call delete on itself from one of its actor methods because the actor cannot be deleted while executing within an actor call context, in which the runtime has obtained a lock around the actor call to enforce single-threaded access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b1baf-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b1baf-172">Next steps</span></span>
* [<span data-ttu-id="b1baf-173">Aktören timers och påminnelser</span><span class="sxs-lookup"><span data-stu-id="b1baf-173">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="b1baf-174">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="b1baf-174">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="b1baf-175">Aktören återinträde</span><span class="sxs-lookup"><span data-stu-id="b1baf-175">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="b1baf-176">Aktören diagnostik- och prestandaövervakning</span><span class="sxs-lookup"><span data-stu-id="b1baf-176">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="b1baf-177">Aktören API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="b1baf-177">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="b1baf-178">C# exempelkod</span><span class="sxs-lookup"><span data-stu-id="b1baf-178">C# Sample code</span></span>](https://github.com/Azure/servicefabric-samples)
* [<span data-ttu-id="b1baf-179">Java-kodexempel</span><span class="sxs-lookup"><span data-stu-id="b1baf-179">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
