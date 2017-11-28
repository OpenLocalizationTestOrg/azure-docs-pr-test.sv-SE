---
title: "aaaOverview aktören-baserad Azure mikrotjänster livscykeln för hantering av | Microsoft Docs"
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
ms.openlocfilehash: a7926e372449048f0a579c2c58573754a4a82363
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a><span data-ttu-id="2318c-103">Aktören livscykel, automatisk skräpinsamling och manuellt ta bort</span><span class="sxs-lookup"><span data-stu-id="2318c-103">Actor lifecycle, automatic garbage collection, and manual delete</span></span>
<span data-ttu-id="2318c-104">En aktör aktiveras hello första gången ett anrop görs tooany av dess metoder.</span><span class="sxs-lookup"><span data-stu-id="2318c-104">An actor is activated hello first time a call is made tooany of its methods.</span></span> <span data-ttu-id="2318c-105">En aktör är inaktiverad (skräp som samlats in av hello aktörer runtime) om den inte används för en konfigurerbar tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="2318c-105">An actor is deactivated (garbage collected by hello Actors runtime) if it is not used for a configurable period of time.</span></span> <span data-ttu-id="2318c-106">En aktör och dess tillstånd kan också tas bort manuellt när som helst.</span><span class="sxs-lookup"><span data-stu-id="2318c-106">An actor and its state can also be deleted manually at any time.</span></span>

## <a name="actor-activation"></a><span data-ttu-id="2318c-107">Aktören aktivering</span><span class="sxs-lookup"><span data-stu-id="2318c-107">Actor activation</span></span>
<span data-ttu-id="2318c-108">När en aktör aktiveras inträffar hello följande:</span><span class="sxs-lookup"><span data-stu-id="2318c-108">When an actor is activated, hello following occurs:</span></span>

* <span data-ttu-id="2318c-109">När ett samtal kommer för en aktör och ingen sådan redan är aktiv, skapas en ny aktören.</span><span class="sxs-lookup"><span data-stu-id="2318c-109">When a call comes for an actor and one is not already active, a new actor is created.</span></span>
* <span data-ttu-id="2318c-110">Hej aktörstillstånd har lästs in om den underhåller tillstånd.</span><span class="sxs-lookup"><span data-stu-id="2318c-110">hello actor's state is loaded if it's maintaining state.</span></span>
* <span data-ttu-id="2318c-111">Hej `OnActivateAsync` (C#) eller `onActivateAsync` (Java)-metoden (som kan åsidosättas i hello aktören implementering) anropas.</span><span class="sxs-lookup"><span data-stu-id="2318c-111">hello `OnActivateAsync` (C#) or `onActivateAsync` (Java) method (which can be overridden in hello actor implementation) is called.</span></span>
* <span data-ttu-id="2318c-112">hello aktören anses nu aktiv.</span><span class="sxs-lookup"><span data-stu-id="2318c-112">hello actor is now considered active.</span></span>

## <a name="actor-deactivation"></a><span data-ttu-id="2318c-113">Aktören avaktivering</span><span class="sxs-lookup"><span data-stu-id="2318c-113">Actor deactivation</span></span>
<span data-ttu-id="2318c-114">När en aktör inaktiveras inträffar hello följande:</span><span class="sxs-lookup"><span data-stu-id="2318c-114">When an actor is deactivated, hello following occurs:</span></span>

* <span data-ttu-id="2318c-115">När en aktör inte används för vissa tidsperiod, är det bort från hello Active aktörer tabell.</span><span class="sxs-lookup"><span data-stu-id="2318c-115">When an actor is not used for some period of time, it is removed from hello Active Actors table.</span></span>
* <span data-ttu-id="2318c-116">Hej `OnDeactivateAsync` (C#) eller `onDeactivateAsync` (Java)-metoden (som kan åsidosättas i hello aktören implementering) anropas.</span><span class="sxs-lookup"><span data-stu-id="2318c-116">hello `OnDeactivateAsync` (C#) or `onDeactivateAsync` (Java) method (which can be overridden in hello actor implementation) is called.</span></span> <span data-ttu-id="2318c-117">Rensar alla hello timers för hello aktören.</span><span class="sxs-lookup"><span data-stu-id="2318c-117">This clears all hello timers for hello actor.</span></span> <span data-ttu-id="2318c-118">Aktören åtgärder som tillstånd ändringar inte ska anropas från den här metoden.</span><span class="sxs-lookup"><span data-stu-id="2318c-118">Actor operations like state changes should not be called from this method.</span></span>

> [!TIP]
> <span data-ttu-id="2318c-119">hello aktörer Fabric runtime skickar vissa [händelser relaterade tooactor aktivering och inaktivering av](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="2318c-119">hello Fabric Actors runtime emits some [events related tooactor activation and deactivation](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters).</span></span> <span data-ttu-id="2318c-120">De är användbara i diagnostik- och prestandaövervakning.</span><span class="sxs-lookup"><span data-stu-id="2318c-120">They are useful in diagnostics and performance monitoring.</span></span>
>
>

### <a name="actor-garbage-collection"></a><span data-ttu-id="2318c-121">Aktören skräpinsamling</span><span class="sxs-lookup"><span data-stu-id="2318c-121">Actor garbage collection</span></span>
<span data-ttu-id="2318c-122">När en aktör inaktiveras referenser toohello aktören objekt släpps och det kan vara skräpinsamlats normalt av hello CLR common language runtime () eller java virtual machine (JVM) skräpinsamlingen.</span><span class="sxs-lookup"><span data-stu-id="2318c-122">When an actor is deactivated, references toohello actor object are released and it can be garbage collected normally by hello common language runtime (CLR) or java virtual machine (JVM) garbage collector.</span></span> <span data-ttu-id="2318c-123">Skräpinsamling endast rensar hello aktören objekt. Det gör **inte** ta bort tillstånd lagras i hello aktören tillstånd Manager.</span><span class="sxs-lookup"><span data-stu-id="2318c-123">Garbage collection only cleans up hello actor object; it does **not** remove state stored in hello actor's State Manager.</span></span> <span data-ttu-id="2318c-124">hello nästa gång hello aktören har aktiverats, skapas ett nytt aktören objekt och dess tillstånd har återställts.</span><span class="sxs-lookup"><span data-stu-id="2318c-124">hello next time hello actor is activated, a new actor object is created and its state is restored.</span></span>

<span data-ttu-id="2318c-125">Vad räknas som ”används” hello syfte inaktivering och skräpinsamling?</span><span class="sxs-lookup"><span data-stu-id="2318c-125">What counts as “being used” for hello purpose of deactivation and garbage collection?</span></span>

* <span data-ttu-id="2318c-126">Ta emot ett samtal</span><span class="sxs-lookup"><span data-stu-id="2318c-126">Receiving a call</span></span>
* <span data-ttu-id="2318c-127">`IRemindable.ReceiveReminderAsync`metoden anropas (gäller endast om hello aktör använder påminnelser)</span><span class="sxs-lookup"><span data-stu-id="2318c-127">`IRemindable.ReceiveReminderAsync` method being invoked (applicable only if hello actor uses reminders)</span></span>

> [!NOTE]
> <span data-ttu-id="2318c-128">Om hello aktör använder timers och dess timer-återanropet anropas, sker **inte** antal som ”används”.</span><span class="sxs-lookup"><span data-stu-id="2318c-128">if hello actor uses timers and its timer callback is invoked, it does **not** count as "being used".</span></span>
>
>

<span data-ttu-id="2318c-129">Innan vi gå in hello information avaktivering är det viktigt toodefine hello följande villkor:</span><span class="sxs-lookup"><span data-stu-id="2318c-129">Before we go into hello details of deactivation, it is important toodefine hello following terms:</span></span>

* <span data-ttu-id="2318c-130">*Skanna intervall*.</span><span class="sxs-lookup"><span data-stu-id="2318c-130">*Scan interval*.</span></span> <span data-ttu-id="2318c-131">Detta är hello intervall på vilka hello aktörer runtime söker igenom Active aktörer tabellen aktörer kan inaktiveras och skräpinsamlats.</span><span class="sxs-lookup"><span data-stu-id="2318c-131">This is hello interval at which hello Actors runtime scans its Active Actors table for actors that can be deactivated and garbage collected.</span></span> <span data-ttu-id="2318c-132">hello standardvärdet är 1 minut.</span><span class="sxs-lookup"><span data-stu-id="2318c-132">hello default value for this is 1 minute.</span></span>
* <span data-ttu-id="2318c-133">*Inaktivitetstid*.</span><span class="sxs-lookup"><span data-stu-id="2318c-133">*Idle timeout*.</span></span> <span data-ttu-id="2318c-134">Detta är hello tid att en aktör måste tooremain oanvända (inaktiv) innan den kan inaktiveras och skräpinsamlats.</span><span class="sxs-lookup"><span data-stu-id="2318c-134">This is hello amount of time that an actor needs tooremain unused (idle) before it can be deactivated and garbage collected.</span></span> <span data-ttu-id="2318c-135">hello standardvärdet är 60 minuter.</span><span class="sxs-lookup"><span data-stu-id="2318c-135">hello default value for this is 60 minutes.</span></span>

<span data-ttu-id="2318c-136">Normalt behöver inte toochange dessa standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="2318c-136">Typically, you do not need toochange these defaults.</span></span> <span data-ttu-id="2318c-137">Men om det behövs dessa intervall kan ändras via `ActorServiceSettings` när du registrerar din [aktören Service](service-fabric-reliable-actors-platform.md):</span><span class="sxs-lookup"><span data-stu-id="2318c-137">However, if necessary, these intervals can be changed through `ActorServiceSettings` when registering your [Actor Service](service-fabric-reliable-actors-platform.md):</span></span>

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
<span data-ttu-id="2318c-138">Hello aktören runtime håller reda på hello mängden tid som den har varit inaktiv (dvs. inte används) för varje active aktören.</span><span class="sxs-lookup"><span data-stu-id="2318c-138">For each active actor, hello actor runtime keeps track of hello amount of time that it has been idle (i.e. not used).</span></span> <span data-ttu-id="2318c-139">hello aktören runtime kontrollerar alla hello aktörer varje `ScanIntervalInSeconds` toosee om det kan vara skräp samlas in och samlar in om det har varit inaktiv i `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="2318c-139">hello actor runtime checks each of hello actors every `ScanIntervalInSeconds` toosee if it can be garbage collected and collects it if it has been idle for `IdleTimeoutInSeconds`.</span></span>

<span data-ttu-id="2318c-140">Varje gång som en aktör används, är dess inaktivitetstid Återställ too0.</span><span class="sxs-lookup"><span data-stu-id="2318c-140">Anytime an actor is used, its idle time is reset too0.</span></span> <span data-ttu-id="2318c-141">Sedan kan hello aktören kan vara skräpinsamlats endast om den igen är vilande för `IdleTimeoutInSeconds`.</span><span class="sxs-lookup"><span data-stu-id="2318c-141">After this, hello actor can be garbage collected only if it again remains idle for `IdleTimeoutInSeconds`.</span></span> <span data-ttu-id="2318c-142">Återkalla en aktör anses toohave har används om en gränssnittsmetod aktören eller ett aktören påminnelse motanrop körs.</span><span class="sxs-lookup"><span data-stu-id="2318c-142">Recall that an actor is considered toohave been used if either an actor interface method or an actor reminder callback is executed.</span></span> <span data-ttu-id="2318c-143">En aktör är **inte** anses vara toohave har används om dess timer motanrop körs.</span><span class="sxs-lookup"><span data-stu-id="2318c-143">An actor is **not** considered toohave been used if its timer callback is executed.</span></span>

<span data-ttu-id="2318c-144">hello följande diagram visar hello livscykeln för ett enda aktören tooillustrate dessa begrepp.</span><span class="sxs-lookup"><span data-stu-id="2318c-144">hello following diagram shows hello lifecycle of a single actor tooillustrate these concepts.</span></span>

![Exempel på inaktivitetstid][1]

<span data-ttu-id="2318c-146">hello exemplet visar hello effekten av aktören metodanrop, påminnelser och timers på den här aktören hello livstid.</span><span class="sxs-lookup"><span data-stu-id="2318c-146">hello example shows hello impact of actor method calls, reminders, and timers on hello lifetime of this actor.</span></span> <span data-ttu-id="2318c-147">följande punkter om hello exempel hello är värt att nämna:</span><span class="sxs-lookup"><span data-stu-id="2318c-147">hello following points about hello example are worth mentioning:</span></span>

* <span data-ttu-id="2318c-148">ScanInterval och IdleTimeout ställs too5 och 10.</span><span class="sxs-lookup"><span data-stu-id="2318c-148">ScanInterval and IdleTimeout are set too5 and 10 respectively.</span></span> <span data-ttu-id="2318c-149">(Enheter inte roll här, eftersom exemplet är endast tooillustrate hello konceptet.)</span><span class="sxs-lookup"><span data-stu-id="2318c-149">(Units do not matter here, since our purpose is only tooillustrate hello concept.)</span></span>
* <span data-ttu-id="2318c-150">hello genomsökningen efter aktörer toobe skräpinsamlats sker på T = 0, 5, 10, 15, 20, 25, som definieras av hello genomsökning intervall på 5.</span><span class="sxs-lookup"><span data-stu-id="2318c-150">hello scan for actors toobe garbage collected happens at T=0,5,10,15,20,25, as defined by hello scan interval of 5.</span></span>
* <span data-ttu-id="2318c-151">En periodisk timer utlöses vid T = 4, 8, 12, 16, 20, 24, och dess motanrop körs.</span><span class="sxs-lookup"><span data-stu-id="2318c-151">A periodic timer fires at T=4,8,12,16,20,24, and its callback executes.</span></span> <span data-ttu-id="2318c-152">Hello inaktivitetstid av hello aktören påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="2318c-152">It does not impact hello idle time of hello actor.</span></span>
* <span data-ttu-id="2318c-153">En aktören metodanrop på T = 7 återställer hello inaktivitetstid too0 och fördröjningar hello skräpinsamling av hello aktören.</span><span class="sxs-lookup"><span data-stu-id="2318c-153">An actor method call at T=7 resets hello idle time too0 and delays hello garbage collection of hello actor.</span></span>
* <span data-ttu-id="2318c-154">Ett återanrop för aktören påminnelse körs på T = 14 och ytterligare fördröjningar hello skräpinsamling av hello aktören.</span><span class="sxs-lookup"><span data-stu-id="2318c-154">An actor reminder callback executes at T=14 and further delays hello garbage collection of hello actor.</span></span>
* <span data-ttu-id="2318c-155">Under hello skräp samling sökningen T = 25 hello aktören inaktivitetstid slutligen överskrider hello timeout vid inaktivitet för 10 och hello aktören samlas in som skräp.</span><span class="sxs-lookup"><span data-stu-id="2318c-155">During hello garbage collection scan at T=25, hello actor's idle time finally exceeds hello idle timeout of 10, and hello actor is garbage collected.</span></span>

<span data-ttu-id="2318c-156">En aktör blir aldrig skräpinsamlats medan det körs en av dess metoder, oavsett hur lång tid det tar vid körning av den här metoden.</span><span class="sxs-lookup"><span data-stu-id="2318c-156">An actor will never be garbage collected while it is executing one of its methods, no matter how much time is spent in executing that method.</span></span> <span data-ttu-id="2318c-157">Som tidigare nämnts förhindrar hello körning av aktören gränssnittsmetoder och påminnelse återanrop skräpinsamling genom att återställa hello aktören inaktivitetstid too0.</span><span class="sxs-lookup"><span data-stu-id="2318c-157">As mentioned earlier, hello execution of actor interface methods and reminder callbacks prevents garbage collection by resetting hello actor's idle time too0.</span></span> <span data-ttu-id="2318c-158">hello körningen av timer återanrop återställs inte hello inaktivitetstid too0.</span><span class="sxs-lookup"><span data-stu-id="2318c-158">hello execution of timer callbacks does not reset hello idle time too0.</span></span> <span data-ttu-id="2318c-159">Hello skräpinsamling av hello aktören skjuts tills hello timer återanrop har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2318c-159">However, hello garbage collection of hello actor is deferred until hello timer callback has completed execution.</span></span>

## <a name="deleting-actors-and-their-state"></a><span data-ttu-id="2318c-160">Ta bort aktörer och deras tillstånd</span><span class="sxs-lookup"><span data-stu-id="2318c-160">Deleting actors and their state</span></span>
<span data-ttu-id="2318c-161">Skräpinsamling inaktiverade aktörer endast rensar hello aktören objekt, men det tar inte bort data som lagras i en aktör tillstånd Manager.</span><span class="sxs-lookup"><span data-stu-id="2318c-161">Garbage collection of deactivated actors only cleans up hello actor object, but it does not remove data that is stored in an actor's State Manager.</span></span> <span data-ttu-id="2318c-162">När en aktör aktiveras igen, sker dess data tillgängliga tooit via hello Tillståndshanterare.</span><span class="sxs-lookup"><span data-stu-id="2318c-162">When an actor is re-activated, its data is again made available tooit through hello State Manager.</span></span> <span data-ttu-id="2318c-163">I fall där aktörer lagra data i Tillståndshanterare och är inaktiverad men aldrig aktiverats igen vara det nödvändigt tooclean av sina data.</span><span class="sxs-lookup"><span data-stu-id="2318c-163">In cases where actors store data in State Manager and are deactivated but never re-activated, it may be necessary tooclean up their data.</span></span>

<span data-ttu-id="2318c-164">Hej [aktören Service](service-fabric-reliable-actors-platform.md) innehåller en funktion för att ta bort aktörer från en fjärransluten anropare:</span><span class="sxs-lookup"><span data-stu-id="2318c-164">hello [Actor Service](service-fabric-reliable-actors-platform.md) provides a function for deleting actors from a remote caller:</span></span>

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

<span data-ttu-id="2318c-165">Om du tar bort en aktör har hello följande effekter beroende på om huruvida hello aktören är för närvarande är aktiva:</span><span class="sxs-lookup"><span data-stu-id="2318c-165">Deleting an actor has hello following effects depending on whether or not hello actor is currently active:</span></span>

* <span data-ttu-id="2318c-166">**Aktiva aktören**</span><span class="sxs-lookup"><span data-stu-id="2318c-166">**Active Actor**</span></span>
  * <span data-ttu-id="2318c-167">Aktören tas bort från listan med aktiva aktörer och inaktiveras.</span><span class="sxs-lookup"><span data-stu-id="2318c-167">Actor is removed from active actors list and is deactivated.</span></span>
  * <span data-ttu-id="2318c-168">Dess tillstånd tas bort permanent.</span><span class="sxs-lookup"><span data-stu-id="2318c-168">Its state is deleted permanently.</span></span>
* <span data-ttu-id="2318c-169">**Inaktiva aktören**</span><span class="sxs-lookup"><span data-stu-id="2318c-169">**Inactive Actor**</span></span>
  * <span data-ttu-id="2318c-170">Dess tillstånd tas bort permanent.</span><span class="sxs-lookup"><span data-stu-id="2318c-170">Its state is deleted permanently.</span></span>

<span data-ttu-id="2318c-171">Observera att det går inte att anropa en aktör ta bort på sig själv från någon av dess metoder aktören eftersom hello aktören inte kan tas bort körs inom en aktören anropet kontext, i vilken hello runtime har fått ett lås runt hello aktören anropet tooenforce Enkeltrådig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="2318c-171">Note that an actor cannot call delete on itself from one of its actor methods because hello actor cannot be deleted while executing within an actor call context, in which hello runtime has obtained a lock around hello actor call tooenforce single-threaded access.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2318c-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2318c-172">Next steps</span></span>
* [<span data-ttu-id="2318c-173">Aktören timers och påminnelser</span><span class="sxs-lookup"><span data-stu-id="2318c-173">Actor timers and reminders</span></span>](service-fabric-reliable-actors-timers-reminders.md)
* [<span data-ttu-id="2318c-174">Aktören händelser</span><span class="sxs-lookup"><span data-stu-id="2318c-174">Actor events</span></span>](service-fabric-reliable-actors-events.md)
* [<span data-ttu-id="2318c-175">Aktören återinträde</span><span class="sxs-lookup"><span data-stu-id="2318c-175">Actor reentrancy</span></span>](service-fabric-reliable-actors-reentrancy.md)
* [<span data-ttu-id="2318c-176">Aktören diagnostik- och prestandaövervakning</span><span class="sxs-lookup"><span data-stu-id="2318c-176">Actor diagnostics and performance monitoring</span></span>](service-fabric-reliable-actors-diagnostics.md)
* [<span data-ttu-id="2318c-177">Aktören API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="2318c-177">Actor API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="2318c-178">C# exempelkod</span><span class="sxs-lookup"><span data-stu-id="2318c-178">C# Sample code</span></span>](https://github.com/Azure/servicefabric-samples)
* [<span data-ttu-id="2318c-179">Java-kodexempel</span><span class="sxs-lookup"><span data-stu-id="2318c-179">Java Sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
