---
title: "Reliable Actors tillstånd management | Microsoft Docs"
description: "Beskriver hur Reliable Actors tillstånd hanteras, beständiga och replikeras för hög tillgänglighet."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: aca8cf2b94e8b746a5cac6af021c7221a29b7345
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="reliable-actors-state-management"></a><span data-ttu-id="3712f-103">Tillförlitliga aktörer tillståndshantering</span><span class="sxs-lookup"><span data-stu-id="3712f-103">Reliable Actors state management</span></span>
<span data-ttu-id="3712f-104">Reliable Actors är enkeltrådad objekt som kan kapsla både logik och tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3712f-104">Reliable Actors are single-threaded objects that can encapsulate both logic and state.</span></span> <span data-ttu-id="3712f-105">Eftersom aktörer körs på Reliable Services, upprätthålla de tillstånd på ett tillförlitligt sätt med hjälp av samma beständiga och replikeringsmekanismer Reliable Services använder.</span><span class="sxs-lookup"><span data-stu-id="3712f-105">Because actors run on Reliable Services, they can maintain state reliably by using the same persistence and replication mechanisms that Reliable Services uses.</span></span> <span data-ttu-id="3712f-106">Det här sättet aktörer förlorar inte deras tillstånd efter fel på återaktivering efter skräpinsamling eller när de flyttas mellan noder i ett kluster på grund av resursen NLB eller uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="3712f-106">This way, actors don't lose their state after failures, upon reactivation after garbage collection, or when they are moved around between nodes in a cluster due to resource balancing or upgrades.</span></span>

## <a name="state-persistence-and-replication"></a><span data-ttu-id="3712f-107">Tillstånd beständighet och replikering</span><span class="sxs-lookup"><span data-stu-id="3712f-107">State persistence and replication</span></span>
<span data-ttu-id="3712f-108">Alla Reliable Actors anses *stateful* eftersom varje instans aktören mappas till ett unikt ID.</span><span class="sxs-lookup"><span data-stu-id="3712f-108">All Reliable Actors are considered *stateful* because each actor instance maps to a unique ID.</span></span> <span data-ttu-id="3712f-109">Det innebär att upprepade anrop till samma aktörs-ID dirigeras till samma aktören-instans.</span><span class="sxs-lookup"><span data-stu-id="3712f-109">This means that repeated calls to the same actor ID are routed to the same actor instance.</span></span> <span data-ttu-id="3712f-110">I ett system med tillståndslös däremot är klientanrop inte säkert att dirigeras till samma server varje gång.</span><span class="sxs-lookup"><span data-stu-id="3712f-110">In a stateless system, by contrast, client calls are not guaranteed to be routed to the same server every time.</span></span> <span data-ttu-id="3712f-111">Därför är aktörstjänster alltid tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="3712f-111">For this reason, actor services are always stateful services.</span></span>

<span data-ttu-id="3712f-112">Även om stateful som de måste lagra tillstånd på ett tillförlitligt sätt inte innebär betraktas som aktörer.</span><span class="sxs-lookup"><span data-stu-id="3712f-112">Even though actors are considered stateful, that does not mean they must store state reliably.</span></span> <span data-ttu-id="3712f-113">Aktörer kan välja vilken beständighet och replikering baserat på deras data lagringskraven:</span><span class="sxs-lookup"><span data-stu-id="3712f-113">Actors can choose the level of state persistence and replication based on their data storage requirements:</span></span>

* <span data-ttu-id="3712f-114">**Beständiga tillståndet**: tillstånd sparat på disken och replikeras till 3 eller fler repliker.</span><span class="sxs-lookup"><span data-stu-id="3712f-114">**Persisted state**: State is persisted to disk and is replicated to 3 or more replicas.</span></span> <span data-ttu-id="3712f-115">Detta är det mest beständiga tillståndet lagringsalternativet där tillstånd kan spara genom hela klustret avbrott.</span><span class="sxs-lookup"><span data-stu-id="3712f-115">This is the most durable state storage option, where state can persist through complete cluster outage.</span></span>
* <span data-ttu-id="3712f-116">**Temporära tillstånd**: tillstånd replikeras till 3 eller fler repliker och endast lagras i minnet.</span><span class="sxs-lookup"><span data-stu-id="3712f-116">**Volatile state**: State is replicated to 3 or more replicas and only kept in memory.</span></span> <span data-ttu-id="3712f-117">Detta ger återhämtning mot nodfel och aktören fel, och under uppgraderingar och resursen belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="3712f-117">This provides resilience against node failure and actor failure, and during upgrades and resource balancing.</span></span> <span data-ttu-id="3712f-118">Dock tillstånd sparas inte till disk.</span><span class="sxs-lookup"><span data-stu-id="3712f-118">However, state is not persisted to disk.</span></span> <span data-ttu-id="3712f-119">Så om alla repliker går förlorade på samma gång, tillståndet förloras samt.</span><span class="sxs-lookup"><span data-stu-id="3712f-119">So if all replicas are lost at once, the state is lost as well.</span></span>
* <span data-ttu-id="3712f-120">**Inga beständiga tillståndet**: tillstånd är inte replikerade eller skrivits till disk.</span><span class="sxs-lookup"><span data-stu-id="3712f-120">**No persisted state**: State is not replicated or written to disk.</span></span> <span data-ttu-id="3712f-121">Den här nivån är för aktörer som bara inte behöver upprätthålla tillstånd på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="3712f-121">This level is for actors that simply don't need to maintain state reliably.</span></span>

<span data-ttu-id="3712f-122">Varje nivå av beständiga är helt enkelt en annan *tillståndsprovidern* och *replikering* konfigurationen av din tjänst.</span><span class="sxs-lookup"><span data-stu-id="3712f-122">Each level of persistence is simply a different *state provider* and *replication* configuration of your service.</span></span> <span data-ttu-id="3712f-123">Om tillståndet skrivs till disk är beroende av tillståndsprovidern - komponenten i en tillförlitlig tjänst som lagrar tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3712f-123">Whether or not state is written to disk depends on the state provider--the component in a reliable service that stores state.</span></span> <span data-ttu-id="3712f-124">Replikeringen beror på hur många repliker för en tjänst har distribuerats med.</span><span class="sxs-lookup"><span data-stu-id="3712f-124">Replication depends on how many replicas a service is deployed with.</span></span> <span data-ttu-id="3712f-125">Precis som med Reliable Services kan både tillståndsprovidern och replikantalet enkelt ställas in manuellt.</span><span class="sxs-lookup"><span data-stu-id="3712f-125">As with Reliable Services, both the state provider and replica count can easily be set manually.</span></span> <span data-ttu-id="3712f-126">Aktören framework innehåller ett attribut som, när används på en aktör, väljs automatiskt en standardprovider för tillstånd och genererar automatiskt inställningarna för replikantalet för att uppnå ett av dessa tre beständiga inställningar.</span><span class="sxs-lookup"><span data-stu-id="3712f-126">The actor framework provides an attribute that, when used on an actor, automatically selects a default state provider and automatically generates settings for replica count to achieve one of these three persistence settings.</span></span> <span data-ttu-id="3712f-127">Attributet StatePersistence ärvs inte av härledd klass, varje aktören måste tillhandahålla StatePersistence nivån.</span><span class="sxs-lookup"><span data-stu-id="3712f-127">The StatePersistence attribute is not inherited by derived class, each Actor type must provide its StatePersistence level.</span></span>

### <a name="persisted-state"></a><span data-ttu-id="3712f-128">Beständiga tillståndet</span><span class="sxs-lookup"><span data-stu-id="3712f-128">Persisted state</span></span>
```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl  extends FabricActor implements MyActor
{
}
```  
<span data-ttu-id="3712f-129">Inställningen använder en tillståndsprovidern som lagrar data på disken och anger automatiskt replikantalet tjänsten till 3.</span><span class="sxs-lookup"><span data-stu-id="3712f-129">This setting uses a state provider that stores data on disk and automatically sets the service replica count to 3.</span></span>

### <a name="volatile-state"></a><span data-ttu-id="3712f-130">Temporära tillstånd</span><span class="sxs-lookup"><span data-stu-id="3712f-130">Volatile state</span></span>
```csharp
[StatePersistence(StatePersistence.Volatile)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Volatile)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
<span data-ttu-id="3712f-131">Den här inställningen används en provider i minne – endast och replikantalet till 3.</span><span class="sxs-lookup"><span data-stu-id="3712f-131">This setting uses an in-memory-only state provider and sets the replica count to 3.</span></span>

### <a name="no-persisted-state"></a><span data-ttu-id="3712f-132">Inga sparade tillstånd</span><span class="sxs-lookup"><span data-stu-id="3712f-132">No persisted state</span></span>
```csharp
[StatePersistence(StatePersistence.None)]
class MyActor : Actor, IMyActor
{
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.None)
class MyActorImpl extends FabricActor implements MyActor
{
}
```
<span data-ttu-id="3712f-133">Den här inställningen används en provider i minne – endast och replikantalet till 1.</span><span class="sxs-lookup"><span data-stu-id="3712f-133">This setting uses an in-memory-only state provider and sets the replica count to 1.</span></span>

### <a name="defaults-and-generated-settings"></a><span data-ttu-id="3712f-134">Standardvärden och genererade inställningar</span><span class="sxs-lookup"><span data-stu-id="3712f-134">Defaults and generated settings</span></span>
<span data-ttu-id="3712f-135">När du använder den `StatePersistence` attribut, en tillståndsprovidern väljs automatiskt för dig vid körning när tjänsten aktören startar.</span><span class="sxs-lookup"><span data-stu-id="3712f-135">When you're using the `StatePersistence` attribute, a state provider is automatically selected for you at runtime when the actor service starts.</span></span> <span data-ttu-id="3712f-136">Replikantalet dock anges vid kompilering av Visual Studio aktören build tools.</span><span class="sxs-lookup"><span data-stu-id="3712f-136">The replica count, however, is set at compile time by the Visual Studio actor build tools.</span></span> <span data-ttu-id="3712f-137">Build-verktyg automatiskt generera ett *standardtjänst* för tjänsten aktören i ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="3712f-137">The build tools automatically generate a *default service* for the actor service in ApplicationManifest.xml.</span></span> <span data-ttu-id="3712f-138">Parametrar skapas för **min replikuppsättningen storlek** och **mål replikuppsättningen storlek**.</span><span class="sxs-lookup"><span data-stu-id="3712f-138">Parameters are created for **min replica set size** and **target replica set size**.</span></span>

<span data-ttu-id="3712f-139">Du kan ändra parametrarna manuellt.</span><span class="sxs-lookup"><span data-stu-id="3712f-139">You can change these parameters manually.</span></span> <span data-ttu-id="3712f-140">Men varje gång den `StatePersistence` attribut ändras, parametrarna är inställda på replik set storlek standardvärden för den valda `StatePersistence` attribut, åsidosätter eventuella tidigare värden.</span><span class="sxs-lookup"><span data-stu-id="3712f-140">But each time the `StatePersistence` attribute is changed, the parameters are set to the default replica set size values for the selected `StatePersistence` attribute, overriding any previous values.</span></span> <span data-ttu-id="3712f-141">Med andra ord värdena som du anger i ServiceManifest.xml är *endast* åsidosättas vid byggning när du ändrar den `StatePersistence` attributvärdet.</span><span class="sxs-lookup"><span data-stu-id="3712f-141">In other words, the values that you set in ServiceManifest.xml are *only* overridden at build time when you change the `StatePersistence` attribute value.</span></span>

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application12Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="MyActorService_PartitionCount" DefaultValue="10" />
      <Parameter Name="MyActorService_MinReplicaSetSize" DefaultValue="3" />
      <Parameter Name="MyActorService_TargetReplicaSetSize" DefaultValue="3" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyActorPkg" ServiceManifestVersion="1.0.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MyActorService" GeneratedIdRef="77d965dc-85fb-488c-bd06-c6c1fe29d593|Persisted">
         <StatefulService ServiceTypeName="MyActorServiceType" TargetReplicaSetSize="[MyActorService_TargetReplicaSetSize]" MinReplicaSetSize="[MyActorService_MinReplicaSetSize]">
            <UniformInt64Partition PartitionCount="[MyActorService_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
         </StatefulService>
      </Service>
   </DefaultServices>
</ApplicationManifest>
```

## <a name="state-manager"></a><span data-ttu-id="3712f-142">Tillstånd manager</span><span class="sxs-lookup"><span data-stu-id="3712f-142">State manager</span></span>
<span data-ttu-id="3712f-143">Varje instans aktören har sin egen tillståndshanterare: en ordlista-liknande datastruktur som lagras på ett tillförlitligt sätt nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="3712f-143">Every actor instance has its own state manager: a dictionary-like data structure that reliably stores key/value pairs.</span></span> <span data-ttu-id="3712f-144">Tillståndshanterarens är en omslutning runt en tillståndsprovidern.</span><span class="sxs-lookup"><span data-stu-id="3712f-144">The state manager is a wrapper around a state provider.</span></span> <span data-ttu-id="3712f-145">Du kan använda den för att lagra data oavsett vilket beständiga inställningen används.</span><span class="sxs-lookup"><span data-stu-id="3712f-145">You can use it to store data regardless of which persistence setting is used.</span></span> <span data-ttu-id="3712f-146">Den ger inte några garantier att en pågående tjänst aktören kan ändras från en inställning för temporär (i minnet-endast) tillstånd till en inställning för beständiga tillståndet med en rullande uppgradering och behålla data.</span><span class="sxs-lookup"><span data-stu-id="3712f-146">It does not provide any guarantees that a running actor service can be changed from a volatile (in-memory-only) state setting to a persisted state setting through a rolling upgrade while preserving data.</span></span> <span data-ttu-id="3712f-147">Det är dock möjligt att ändra replikantalet för en tjänst som körs.</span><span class="sxs-lookup"><span data-stu-id="3712f-147">However, it is possible to change replica count for a running service.</span></span>

<span data-ttu-id="3712f-148">Tillstånd manager nycklar måste vara strängar.</span><span class="sxs-lookup"><span data-stu-id="3712f-148">State manager keys must be strings.</span></span> <span data-ttu-id="3712f-149">Värden är generisk och kan ha olika typer, inklusive anpassade typer.</span><span class="sxs-lookup"><span data-stu-id="3712f-149">Values are generic and can be any type, including custom types.</span></span> <span data-ttu-id="3712f-150">Värden som lagras i tillståndshanterarens måste vara datakontrakt serialiserbara eftersom de kan överföras via nätverket till andra noder vid replikering och kan skrivas till disk, beroende på en aktör tillståndsinställningen persistence.</span><span class="sxs-lookup"><span data-stu-id="3712f-150">Values stored in the state manager must be data contract serializable because they might be transmitted over the network to other nodes during replication and might be written to disk, depending on an actor's state persistence setting.</span></span>

<span data-ttu-id="3712f-151">Tillståndshanterarens visar vanliga ordlista metoder för att hantera tillstånd, liknar de som finns i tillförlitliga ordlistan.</span><span class="sxs-lookup"><span data-stu-id="3712f-151">The state manager exposes common dictionary methods for managing state, similar to those found in Reliable Dictionary.</span></span>

### <a name="accessing-state"></a><span data-ttu-id="3712f-152">Åtkomst till tillstånd</span><span class="sxs-lookup"><span data-stu-id="3712f-152">Accessing state</span></span>
<span data-ttu-id="3712f-153">Tillstånd kan nås via tillståndshanterarens av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="3712f-153">State can be accessed through the state manager by key.</span></span> <span data-ttu-id="3712f-154">Tillstånd manager metoder är alla asynkrona eftersom de kan kräva disk-i/o när aktörer beständiga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3712f-154">State manager methods are all asynchronous because they might require disk I/O when actors have persisted state.</span></span> <span data-ttu-id="3712f-155">Vid första åtkomst cachelagras tillstånd objekt i minnet.</span><span class="sxs-lookup"><span data-stu-id="3712f-155">Upon first access, state objects are cached in memory.</span></span> <span data-ttu-id="3712f-156">Upprepa åtkomst operations access-objekt direkt från minnet och returnera synkront utan att det medför i/o eller asynkrona kontexten-växla omkostnader.</span><span class="sxs-lookup"><span data-stu-id="3712f-156">Repeat access operations access objects directly from memory and return synchronously without incurring disk I/O or asynchronous context-switching overhead.</span></span> <span data-ttu-id="3712f-157">Ett tillstånd objekt tas bort från cachen i följande fall:</span><span class="sxs-lookup"><span data-stu-id="3712f-157">A state object is removed from the cache in the following cases:</span></span>

* <span data-ttu-id="3712f-158">En aktörsmetod utlöser ett undantag när det hämtar ett objekt från hanteraren tillstånd.</span><span class="sxs-lookup"><span data-stu-id="3712f-158">An actor method throws an unhandled exception after it retrieves an object from the state manager.</span></span>
* <span data-ttu-id="3712f-159">En aktör återaktiveras när de har inaktiverats eller efter fel.</span><span class="sxs-lookup"><span data-stu-id="3712f-159">An actor is reactivated, either after being deactivated or after failure.</span></span>
* <span data-ttu-id="3712f-160">Tillstånd providern sidor status till disk.</span><span class="sxs-lookup"><span data-stu-id="3712f-160">The state provider pages state to disk.</span></span> <span data-ttu-id="3712f-161">Det här problemet beror på tillstånd providerns implementering.</span><span class="sxs-lookup"><span data-stu-id="3712f-161">This behavior depends on the state provider implementation.</span></span> <span data-ttu-id="3712f-162">Standardprovidern för tillstånd för den `Persisted` har problemet.</span><span class="sxs-lookup"><span data-stu-id="3712f-162">The default state provider for the `Persisted` setting has this behavior.</span></span>

<span data-ttu-id="3712f-163">Du kan hämta tillstånd genom att använda ett standard- *hämta* åtgärden utlöser `KeyNotFoundException`(C#) eller `NoSuchElementException`(Java) om det inte finns en post för nyckeln:</span><span class="sxs-lookup"><span data-stu-id="3712f-163">You can retrieve state by using a standard *Get* operation that throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) if an entry does not exist for the key:</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

<span data-ttu-id="3712f-164">Du kan också hämta tillstånd med hjälp av en *TryGet* metod som inte genereras om det inte finns en post för en nyckel:</span><span class="sxs-lookup"><span data-stu-id="3712f-164">You can also retrieve state by using a *TryGet* method that does not throw if an entry does not exist for a key:</span></span>

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

### <a name="saving-state"></a><span data-ttu-id="3712f-165">Sparar tillstånd</span><span class="sxs-lookup"><span data-stu-id="3712f-165">Saving state</span></span>
<span data-ttu-id="3712f-166">Tillstånd manager hämtning metoder returnerar en referens till ett objekt i lokalt minne.</span><span class="sxs-lookup"><span data-stu-id="3712f-166">The state manager retrieval methods return a reference to an object in local memory.</span></span> <span data-ttu-id="3712f-167">Ändra det här objektet i lokala minnet enbart orsakar inte den sparas varaktigt.</span><span class="sxs-lookup"><span data-stu-id="3712f-167">Modifying this object in local memory alone does not cause it to be saved durably.</span></span> <span data-ttu-id="3712f-168">När ett objekt som hämtats från tillståndshanterarens och ändras, måste den igen i tillståndshanterarens sparas varaktigt.</span><span class="sxs-lookup"><span data-stu-id="3712f-168">When an object is retrieved from the state manager and modified, it must be reinserted into the state manager to be saved durably.</span></span>

<span data-ttu-id="3712f-169">Du kan infoga tillstånd med hjälp av en ovillkorlig *ange*, som motsvarar den `dictionary["key"] = value` syntax:</span><span class="sxs-lookup"><span data-stu-id="3712f-169">You can insert state by using an unconditional *Set*, which is the equivalent of the `dictionary["key"] = value` syntax:</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

<span data-ttu-id="3712f-170">Du kan lägga till tillstånd med hjälp av en *Lägg till* metod.</span><span class="sxs-lookup"><span data-stu-id="3712f-170">You can add state by using an *Add* method.</span></span> <span data-ttu-id="3712f-171">Den här metoden genererar `InvalidOperationException`(C#) eller `IllegalStateException`(Java) vid försök att lägga till en nyckel som redan finns.</span><span class="sxs-lookup"><span data-stu-id="3712f-171">This method throws `InvalidOperationException`(C#) or `IllegalStateException`(Java) when it tries to add a key that already exists.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

<span data-ttu-id="3712f-172">Du kan också lägga till tillstånd med hjälp av en *TryAdd* metod.</span><span class="sxs-lookup"><span data-stu-id="3712f-172">You can also add state by using a *TryAdd* method.</span></span> <span data-ttu-id="3712f-173">Den här metoden genereras inget när den försöker lägga till en nyckel som redan finns.</span><span class="sxs-lookup"><span data-stu-id="3712f-173">This method does not throw when it tries to add a key that already exists.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

<span data-ttu-id="3712f-174">I slutet av en aktörsmetod sparar tillståndshanterarens automatiskt alla värden som har lagts till eller ändrats av en insert eller update-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3712f-174">At the end of an actor method, the state manager automatically saves any values that have been added or modified by an insert or update operation.</span></span> <span data-ttu-id="3712f-175">”Spara” kan innehålla beständighet till disk och replikering, beroende på inställningarna som används.</span><span class="sxs-lookup"><span data-stu-id="3712f-175">A "save" can include persisting to disk and replication, depending on the settings used.</span></span> <span data-ttu-id="3712f-176">Värden som inte har ändrats beständiga eller replikeras inte.</span><span class="sxs-lookup"><span data-stu-id="3712f-176">Values that have not been modified are not persisted or replicated.</span></span> <span data-ttu-id="3712f-177">Om inga värden har ändrats, spara åtgärden händer ingenting.</span><span class="sxs-lookup"><span data-stu-id="3712f-177">If no values have been modified, the save operation does nothing.</span></span> <span data-ttu-id="3712f-178">Om du sparar misslyckas ändringstillstånd ignoreras och det ursprungliga tillståndet läses.</span><span class="sxs-lookup"><span data-stu-id="3712f-178">If saving fails, the modified state is discarded and the original state is reloaded.</span></span>

<span data-ttu-id="3712f-179">Du kan också spara tillstånd manuellt genom att anropa den `SaveStateAsync` metod i basen aktören:</span><span class="sxs-lookup"><span data-stu-id="3712f-179">You can also save state manually by calling the `SaveStateAsync` method on the actor base:</span></span>

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

### <a name="removing-state"></a><span data-ttu-id="3712f-180">Ta bort tillstånd</span><span class="sxs-lookup"><span data-stu-id="3712f-180">Removing state</span></span>
<span data-ttu-id="3712f-181">Du kan ta bort tillstånd permanent från en aktör tillstånd manager genom att anropa den *ta bort* metod.</span><span class="sxs-lookup"><span data-stu-id="3712f-181">You can remove state permanently from an actor's state manager by calling the *Remove* method.</span></span> <span data-ttu-id="3712f-182">Den här metoden genererar `KeyNotFoundException`(C#) eller `NoSuchElementException`(Java) vid försök att ta bort en nyckel som inte finns.</span><span class="sxs-lookup"><span data-stu-id="3712f-182">This method throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) when it tries to remove a key that doesn't exist.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

<span data-ttu-id="3712f-183">Du kan också ta bort tillstånd permanent med hjälp av den *TryRemove* metod.</span><span class="sxs-lookup"><span data-stu-id="3712f-183">You can also remove state permanently by using the *TryRemove* method.</span></span> <span data-ttu-id="3712f-184">Den här metoden genereras inget när den försöker ta bort en nyckel som inte finns.</span><span class="sxs-lookup"><span data-stu-id="3712f-184">This method does not throw when it tries to remove a key that does not exist.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="3712f-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3712f-185">Next steps</span></span>

<span data-ttu-id="3712f-186">Tillstånd som är lagrad i Reliable Actors måste serialiseras innan dess skrivits till disk och replikeras för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="3712f-186">State that's stored in Reliable Actors must be serialized before its written to disk and replicated for high availability.</span></span> <span data-ttu-id="3712f-187">Lär dig mer om [aktören typserialiseringen](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="3712f-187">Learn more about [Actor type serialization](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span>

<span data-ttu-id="3712f-188">Därefter lär dig mer om [aktören diagnostik- och prestandaövervakning](service-fabric-reliable-actors-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="3712f-188">Next, learn more about [Actor diagnostics and performance monitoring](service-fabric-reliable-actors-diagnostics.md).</span></span>
