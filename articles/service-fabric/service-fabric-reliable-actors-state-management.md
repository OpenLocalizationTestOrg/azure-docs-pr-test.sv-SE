---
title: "aaaReliable aktörer tillstånd management | Microsoft Docs"
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
ms.openlocfilehash: 346d92426b1890617d108a9504afb179e463bded
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-actors-state-management"></a><span data-ttu-id="e4d9d-103">Tillförlitliga aktörer tillståndshantering</span><span class="sxs-lookup"><span data-stu-id="e4d9d-103">Reliable Actors state management</span></span>
<span data-ttu-id="e4d9d-104">Reliable Actors är enkeltrådad objekt som kan kapsla både logik och tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-104">Reliable Actors are single-threaded objects that can encapsulate both logic and state.</span></span> <span data-ttu-id="e4d9d-105">Eftersom aktörer körs på Reliable Services, de upprätthålla tillstånd på ett tillförlitligt sätt med hjälp av hello samma beständighet och replikering mekanismer som Reliable Services använder.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-105">Because actors run on Reliable Services, they can maintain state reliably by using hello same persistence and replication mechanisms that Reliable Services uses.</span></span> <span data-ttu-id="e4d9d-106">Det här sättet aktörer förlorar inte deras tillstånd efter fel på återaktivering efter skräpinsamling eller när de flyttas mellan noder i ett kluster på grund av tooresource NLB eller uppgraderingar.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-106">This way, actors don't lose their state after failures, upon reactivation after garbage collection, or when they are moved around between nodes in a cluster due tooresource balancing or upgrades.</span></span>

## <a name="state-persistence-and-replication"></a><span data-ttu-id="e4d9d-107">Tillstånd beständighet och replikering</span><span class="sxs-lookup"><span data-stu-id="e4d9d-107">State persistence and replication</span></span>
<span data-ttu-id="e4d9d-108">Alla Reliable Actors anses *stateful* eftersom varje instans aktören mappar tooa unikt ID.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-108">All Reliable Actors are considered *stateful* because each actor instance maps tooa unique ID.</span></span> <span data-ttu-id="e4d9d-109">Det innebär att upprepade anrop toohello samma aktörs-ID är dirigeras toohello samma aktören-instans.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-109">This means that repeated calls toohello same actor ID are routed toohello same actor instance.</span></span> <span data-ttu-id="e4d9d-110">I ett system med tillståndslös däremot klientanrop är inte garanterat toobe dirigeras toohello samma server varje gång.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-110">In a stateless system, by contrast, client calls are not guaranteed toobe routed toohello same server every time.</span></span> <span data-ttu-id="e4d9d-111">Därför är aktörstjänster alltid tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-111">For this reason, actor services are always stateful services.</span></span>

<span data-ttu-id="e4d9d-112">Även om stateful som de måste lagra tillstånd på ett tillförlitligt sätt inte innebär betraktas som aktörer.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-112">Even though actors are considered stateful, that does not mean they must store state reliably.</span></span> <span data-ttu-id="e4d9d-113">Aktörer kan välja hello beständighet och replikering baserat på deras data lagringskraven:</span><span class="sxs-lookup"><span data-stu-id="e4d9d-113">Actors can choose hello level of state persistence and replication based on their data storage requirements:</span></span>

* <span data-ttu-id="e4d9d-114">**Beständiga tillståndet**: tillstånd är beständiga toodisk och replikerade too3 eller flera repliker.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-114">**Persisted state**: State is persisted toodisk and is replicated too3 or more replicas.</span></span> <span data-ttu-id="e4d9d-115">Detta är hello mest beständiga tillståndet lagringsalternativ, där tillståndet kan spara genom hela klustret avbrott.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-115">This is hello most durable state storage option, where state can persist through complete cluster outage.</span></span>
* <span data-ttu-id="e4d9d-116">**Temporära tillstånd**: tillstånd är replikerade too3 eller flera repliker och endast lagras i minnet.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-116">**Volatile state**: State is replicated too3 or more replicas and only kept in memory.</span></span> <span data-ttu-id="e4d9d-117">Detta ger återhämtning mot nodfel och aktören fel, och under uppgraderingar och resursen belastningsutjämning.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-117">This provides resilience against node failure and actor failure, and during upgrades and resource balancing.</span></span> <span data-ttu-id="e4d9d-118">Dock är tillstånd inte beständiga toodisk.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-118">However, state is not persisted toodisk.</span></span> <span data-ttu-id="e4d9d-119">Så om alla repliker går förlorade på samma gång, går hello tillstånd förlorad samt.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-119">So if all replicas are lost at once, hello state is lost as well.</span></span>
* <span data-ttu-id="e4d9d-120">**Inga beständiga tillståndet**: tillstånd är inte replikeras eller skrivs toodisk.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-120">**No persisted state**: State is not replicated or written toodisk.</span></span> <span data-ttu-id="e4d9d-121">Den här nivån är för aktörer som bara behöver toomaintain tillstånd på ett tillförlitligt sätt.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-121">This level is for actors that simply don't need toomaintain state reliably.</span></span>

<span data-ttu-id="e4d9d-122">Varje nivå av beständiga är helt enkelt en annan *tillståndsprovidern* och *replikering* konfigurationen av din tjänst.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-122">Each level of persistence is simply a different *state provider* and *replication* configuration of your service.</span></span> <span data-ttu-id="e4d9d-123">Oavsett om tillståndet är skriven beroende toodisk hello tillståndsprovidern--hello-komponenten i en tillförlitlig tjänst som lagrar tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-123">Whether or not state is written toodisk depends on hello state provider--hello component in a reliable service that stores state.</span></span> <span data-ttu-id="e4d9d-124">Replikeringen beror på hur många repliker för en tjänst har distribuerats med.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-124">Replication depends on how many replicas a service is deployed with.</span></span> <span data-ttu-id="e4d9d-125">Precis som med Reliable Services både hello tillståndsprovidern och kan enkelt ställas in replikantalet manuellt.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-125">As with Reliable Services, both hello state provider and replica count can easily be set manually.</span></span> <span data-ttu-id="e4d9d-126">hello aktören framework innehåller ett attribut att när används på en aktör väljer automatiskt en standardprovider för tillstånd och inställningar för replik antal tooachieve någon av dessa tre beständiga inställningar genereras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-126">hello actor framework provides an attribute that, when used on an actor, automatically selects a default state provider and automatically generates settings for replica count tooachieve one of these three persistence settings.</span></span> <span data-ttu-id="e4d9d-127">Hej StatePersistence attributet ärvs inte av härledd klass, varje aktören måste tillhandahålla StatePersistence nivån.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-127">hello StatePersistence attribute is not inherited by derived class, each Actor type must provide its StatePersistence level.</span></span>

### <a name="persisted-state"></a><span data-ttu-id="e4d9d-128">Beständiga tillståndet</span><span class="sxs-lookup"><span data-stu-id="e4d9d-128">Persisted state</span></span>
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
<span data-ttu-id="e4d9d-129">Inställningen använder en tillståndsprovidern som lagrar data på disken och anger hello service replik antal too3 automatiskt.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-129">This setting uses a state provider that stores data on disk and automatically sets hello service replica count too3.</span></span>

### <a name="volatile-state"></a><span data-ttu-id="e4d9d-130">Temporära tillstånd</span><span class="sxs-lookup"><span data-stu-id="e4d9d-130">Volatile state</span></span>
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
<span data-ttu-id="e4d9d-131">Den här inställningen använder en provider i minne – endast och anger hello replik antal too3.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-131">This setting uses an in-memory-only state provider and sets hello replica count too3.</span></span>

### <a name="no-persisted-state"></a><span data-ttu-id="e4d9d-132">Inga sparade tillstånd</span><span class="sxs-lookup"><span data-stu-id="e4d9d-132">No persisted state</span></span>
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
<span data-ttu-id="e4d9d-133">Den här inställningen använder en provider i minne – endast och anger hello replik antal too1.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-133">This setting uses an in-memory-only state provider and sets hello replica count too1.</span></span>

### <a name="defaults-and-generated-settings"></a><span data-ttu-id="e4d9d-134">Standardvärden och genererade inställningar</span><span class="sxs-lookup"><span data-stu-id="e4d9d-134">Defaults and generated settings</span></span>
<span data-ttu-id="e4d9d-135">När du använder hello `StatePersistence` attribut, en tillståndsprovidern väljs automatiskt för dig vid körning när hello aktören-tjänsten startas.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-135">When you're using hello `StatePersistence` attribute, a state provider is automatically selected for you at runtime when hello actor service starts.</span></span> <span data-ttu-id="e4d9d-136">Hej replikantalet dock anges vid kompilering av hello Visual Studio aktören verktyg.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-136">hello replica count, however, is set at compile time by hello Visual Studio actor build tools.</span></span> <span data-ttu-id="e4d9d-137">hello verktyg automatiskt generera ett *standardtjänst* hello aktören tjänst i ApplicationManifest.xml.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-137">hello build tools automatically generate a *default service* for hello actor service in ApplicationManifest.xml.</span></span> <span data-ttu-id="e4d9d-138">Parametrar skapas för **min replikuppsättningen storlek** och **mål replikuppsättningen storlek**.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-138">Parameters are created for **min replica set size** and **target replica set size**.</span></span>

<span data-ttu-id="e4d9d-139">Du kan ändra parametrarna manuellt.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-139">You can change these parameters manually.</span></span> <span data-ttu-id="e4d9d-140">Men varje gång hello `StatePersistence` attribut ändras, hello har angetts toohello replik set storlek standardvärden för hello valt `StatePersistence` attribut, åsidosätter eventuella tidigare värden.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-140">But each time hello `StatePersistence` attribute is changed, hello parameters are set toohello default replica set size values for hello selected `StatePersistence` attribute, overriding any previous values.</span></span> <span data-ttu-id="e4d9d-141">Med andra ord hello värdena som du anger i ServiceManifest.xml är *endast* åsidosättas vid byggning när du ändrar hello `StatePersistence` attributvärdet.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-141">In other words, hello values that you set in ServiceManifest.xml are *only* overridden at build time when you change hello `StatePersistence` attribute value.</span></span>

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

## <a name="state-manager"></a><span data-ttu-id="e4d9d-142">Tillstånd manager</span><span class="sxs-lookup"><span data-stu-id="e4d9d-142">State manager</span></span>
<span data-ttu-id="e4d9d-143">Varje instans aktören har sin egen tillståndshanterare: en ordlista-liknande datastruktur som lagras på ett tillförlitligt sätt nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-143">Every actor instance has its own state manager: a dictionary-like data structure that reliably stores key/value pairs.</span></span> <span data-ttu-id="e4d9d-144">hello tillstånd manager är en omslutning runt en tillståndsprovidern.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-144">hello state manager is a wrapper around a state provider.</span></span> <span data-ttu-id="e4d9d-145">Du kan använda den toostore data oavsett vilken beständiga inställning används.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-145">You can use it toostore data regardless of which persistence setting is used.</span></span> <span data-ttu-id="e4d9d-146">Det ger inte några garantier att en aktiv tjänst aktören kan ändras från en temporär (i minnet-endast) tillstånd inställningen tooa beständiga tillståndsinställningen via en löpande uppgradering och behålla data.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-146">It does not provide any guarantees that a running actor service can be changed from a volatile (in-memory-only) state setting tooa persisted state setting through a rolling upgrade while preserving data.</span></span> <span data-ttu-id="e4d9d-147">Det är dock möjligt toochange replikantalet för en tjänst som körs.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-147">However, it is possible toochange replica count for a running service.</span></span>

<span data-ttu-id="e4d9d-148">Tillstånd manager nycklar måste vara strängar.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-148">State manager keys must be strings.</span></span> <span data-ttu-id="e4d9d-149">Värden är generisk och kan ha olika typer, inklusive anpassade typer.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-149">Values are generic and can be any type, including custom types.</span></span> <span data-ttu-id="e4d9d-150">Värden som lagras i hello tillståndshanterare måste vara datakontrakt serialiserbara eftersom de kan komma att överföras hello tooother nätverksnoder vid replikering och kan skrivas toodisk, beroende på en aktör tillståndsinställningen persistence.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-150">Values stored in hello state manager must be data contract serializable because they might be transmitted over hello network tooother nodes during replication and might be written toodisk, depending on an actor's state persistence setting.</span></span>

<span data-ttu-id="e4d9d-151">Hej tillståndshanterare exponerar vanliga ordlista metoder för att hantera tillstånd, liknande toothose hittades i tillförlitliga ordlistan.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-151">hello state manager exposes common dictionary methods for managing state, similar toothose found in Reliable Dictionary.</span></span>

### <a name="accessing-state"></a><span data-ttu-id="e4d9d-152">Åtkomst till tillstånd</span><span class="sxs-lookup"><span data-stu-id="e4d9d-152">Accessing state</span></span>
<span data-ttu-id="e4d9d-153">Tillstånd kan nås via hello tillståndshanterare av nyckeln.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-153">State can be accessed through hello state manager by key.</span></span> <span data-ttu-id="e4d9d-154">Tillstånd manager metoder är alla asynkrona eftersom de kan kräva disk-i/o när aktörer beständiga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-154">State manager methods are all asynchronous because they might require disk I/O when actors have persisted state.</span></span> <span data-ttu-id="e4d9d-155">Vid första åtkomst cachelagras tillstånd objekt i minnet.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-155">Upon first access, state objects are cached in memory.</span></span> <span data-ttu-id="e4d9d-156">Upprepa åtkomst operations access-objekt direkt från minnet och returnera synkront utan att det medför i/o eller asynkrona kontexten-växla omkostnader.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-156">Repeat access operations access objects directly from memory and return synchronously without incurring disk I/O or asynchronous context-switching overhead.</span></span> <span data-ttu-id="e4d9d-157">Ett tillstånd objekt tas bort från hello-cache i hello följande fall:</span><span class="sxs-lookup"><span data-stu-id="e4d9d-157">A state object is removed from hello cache in hello following cases:</span></span>

* <span data-ttu-id="e4d9d-158">En aktörsmetod utlöser ett undantag när det hämtar ett objekt från hello tillståndshanterare.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-158">An actor method throws an unhandled exception after it retrieves an object from hello state manager.</span></span>
* <span data-ttu-id="e4d9d-159">En aktör återaktiveras när de har inaktiverats eller efter fel.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-159">An actor is reactivated, either after being deactivated or after failure.</span></span>
* <span data-ttu-id="e4d9d-160">hello tillstånd providern sidor tillstånd toodisk.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-160">hello state provider pages state toodisk.</span></span> <span data-ttu-id="e4d9d-161">Det här problemet beror på hello tillstånd providerns implementering.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-161">This behavior depends on hello state provider implementation.</span></span> <span data-ttu-id="e4d9d-162">hello tillstånd standardprovidern för hello `Persisted` har problemet.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-162">hello default state provider for hello `Persisted` setting has this behavior.</span></span>

<span data-ttu-id="e4d9d-163">Du kan hämta tillstånd genom att använda ett standard- *hämta* åtgärden utlöser `KeyNotFoundException`(C#) eller `NoSuchElementException`(Java) om det inte finns en post för hello nyckeln:</span><span class="sxs-lookup"><span data-stu-id="e4d9d-163">You can retrieve state by using a standard *Get* operation that throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) if an entry does not exist for hello key:</span></span>

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

<span data-ttu-id="e4d9d-164">Du kan också hämta tillstånd med hjälp av en *TryGet* metod som inte genereras om det inte finns en post för en nyckel:</span><span class="sxs-lookup"><span data-stu-id="e4d9d-164">You can also retrieve state by using a *TryGet* method that does not throw if an entry does not exist for a key:</span></span>

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

### <a name="saving-state"></a><span data-ttu-id="e4d9d-165">Sparar tillstånd</span><span class="sxs-lookup"><span data-stu-id="e4d9d-165">Saving state</span></span>
<span data-ttu-id="e4d9d-166">metoder för hämtning av hello tillstånd manager returnera ett referensobjekt tooan i lokalt minne.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-166">hello state manager retrieval methods return a reference tooan object in local memory.</span></span> <span data-ttu-id="e4d9d-167">Ändra det här objektet i lokala minnet enbart orsakar inte den toobe sparade varaktigt.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-167">Modifying this object in local memory alone does not cause it toobe saved durably.</span></span> <span data-ttu-id="e4d9d-168">När ett objekt som hämtats från hello tillståndshanterare och ändras, måste den igen i hello tillstånd manager toobe sparade varaktigt.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-168">When an object is retrieved from hello state manager and modified, it must be reinserted into hello state manager toobe saved durably.</span></span>

<span data-ttu-id="e4d9d-169">Du kan infoga tillstånd med hjälp av en ovillkorlig *ange*, vilket är hello motsvarar hello `dictionary["key"] = value` syntax:</span><span class="sxs-lookup"><span data-stu-id="e4d9d-169">You can insert state by using an unconditional *Set*, which is hello equivalent of hello `dictionary["key"] = value` syntax:</span></span>

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

<span data-ttu-id="e4d9d-170">Du kan lägga till tillstånd med hjälp av en *Lägg till* metod.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-170">You can add state by using an *Add* method.</span></span> <span data-ttu-id="e4d9d-171">Den här metoden genererar `InvalidOperationException`(C#) eller `IllegalStateException`(Java) när den försöker tooadd en nyckel som redan finns.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-171">This method throws `InvalidOperationException`(C#) or `IllegalStateException`(Java) when it tries tooadd a key that already exists.</span></span>

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

<span data-ttu-id="e4d9d-172">Du kan också lägga till tillstånd med hjälp av en *TryAdd* metod.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-172">You can also add state by using a *TryAdd* method.</span></span> <span data-ttu-id="e4d9d-173">Den här metoden genereras inget när den försöker tooadd en nyckel som redan finns.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-173">This method does not throw when it tries tooadd a key that already exists.</span></span>

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

<span data-ttu-id="e4d9d-174">Hello slutet av en aktörsmetod sparar hello tillståndshanterare automatiskt alla värden som har lagts till eller ändrats av en insert eller update-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-174">At hello end of an actor method, hello state manager automatically saves any values that have been added or modified by an insert or update operation.</span></span> <span data-ttu-id="e4d9d-175">”Spara” kan innehålla bestående toodisk och replikering, beroende på inställningarna för hello används.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-175">A "save" can include persisting toodisk and replication, depending on hello settings used.</span></span> <span data-ttu-id="e4d9d-176">Värden som inte har ändrats beständiga eller replikeras inte.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-176">Values that have not been modified are not persisted or replicated.</span></span> <span data-ttu-id="e4d9d-177">Om inga värden har ändrats, ingenting hello åtgärden för att spara.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-177">If no values have been modified, hello save operation does nothing.</span></span> <span data-ttu-id="e4d9d-178">Om du sparar misslyckas hello ändrade tillstånd ignoreras och läsas in igen hello ursprungliga tillstånd.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-178">If saving fails, hello modified state is discarded and hello original state is reloaded.</span></span>

<span data-ttu-id="e4d9d-179">Du kan också spara tillstånd manuellt genom att anropa hello `SaveStateAsync` metoden på hello aktören bas:</span><span class="sxs-lookup"><span data-stu-id="e4d9d-179">You can also save state manually by calling hello `SaveStateAsync` method on hello actor base:</span></span>

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

### <a name="removing-state"></a><span data-ttu-id="e4d9d-180">Ta bort tillstånd</span><span class="sxs-lookup"><span data-stu-id="e4d9d-180">Removing state</span></span>
<span data-ttu-id="e4d9d-181">Du kan ta bort tillstånd permanent från en aktör tillstånd manager genom att anropa hello *ta bort* metod.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-181">You can remove state permanently from an actor's state manager by calling hello *Remove* method.</span></span> <span data-ttu-id="e4d9d-182">Den här metoden genererar `KeyNotFoundException`(C#) eller `NoSuchElementException`(Java) när den försöker tooremove en nyckel som inte finns.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-182">This method throws `KeyNotFoundException`(C#) or `NoSuchElementException`(Java) when it tries tooremove a key that doesn't exist.</span></span>

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

<span data-ttu-id="e4d9d-183">Du kan också ta bort tillstånd permanent med hello *TryRemove* metod.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-183">You can also remove state permanently by using hello *TryRemove* method.</span></span> <span data-ttu-id="e4d9d-184">Den här metoden genereras inget när den försöker tooremove en nyckel som inte finns.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-184">This method does not throw when it tries tooremove a key that does not exist.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="e4d9d-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e4d9d-185">Next steps</span></span>

<span data-ttu-id="e4d9d-186">Tillstånd som är lagrad i Reliable Actors måste serialiseras innan dess skriftliga toodisk och replikeras för hög tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="e4d9d-186">State that's stored in Reliable Actors must be serialized before its written toodisk and replicated for high availability.</span></span> <span data-ttu-id="e4d9d-187">Lär dig mer om [aktören typserialiseringen](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span><span class="sxs-lookup"><span data-stu-id="e4d9d-187">Learn more about [Actor type serialization](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).</span></span>

<span data-ttu-id="e4d9d-188">Därefter lär dig mer om [aktören diagnostik- och prestandaövervakning](service-fabric-reliable-actors-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="e4d9d-188">Next, learn more about [Actor diagnostics and performance monitoring](service-fabric-reliable-actors-diagnostics.md).</span></span>
