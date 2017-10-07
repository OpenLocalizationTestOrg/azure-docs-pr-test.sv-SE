---
title: "aaaReliable aktörer på Service Fabric | Microsoft Docs"
description: "Beskriver hur Reliable Actors ovanpå Reliable Services och använda hello funktioner för hello Service Fabric-plattformen."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: ecffb54139f1171c7839b77fed0be60950881198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-reliable-actors-use-hello-service-fabric-platform"></a><span data-ttu-id="a3437-103">Hur Reliable Actors använda hello Service Fabric-plattformen</span><span class="sxs-lookup"><span data-stu-id="a3437-103">How Reliable Actors use hello Service Fabric platform</span></span>
<span data-ttu-id="a3437-104">Den här artikeln förklarar hur Reliable Actors fungerar på hello Azure Service Fabric-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a3437-104">This article explains how Reliable Actors work on hello Azure Service Fabric platform.</span></span> <span data-ttu-id="a3437-105">Reliable Actors köras i ett ramverk som är värd för en implementering av en tillståndskänslig tillförlitlig tjänst som kallas hello *aktören tjänsten*.</span><span class="sxs-lookup"><span data-stu-id="a3437-105">Reliable Actors run in a framework that is hosted in an implementation of a stateful reliable service called hello *actor service*.</span></span> <span data-ttu-id="a3437-106">hello aktören tjänsten innehåller alla hello komponenter nödvändiga toomanage hello livscykel och meddelandet sändning för din aktörer:</span><span class="sxs-lookup"><span data-stu-id="a3437-106">hello actor service contains all hello components necessary toomanage hello lifecycle and message dispatching for your actors:</span></span>

* <span data-ttu-id="a3437-107">hello aktören Runtime hanterar livscykeln skräpinsamling, och tillämpar Enkeltrådig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="a3437-107">hello Actor Runtime manages lifecycle, garbage collection, and enforces single-threaded access.</span></span>
* <span data-ttu-id="a3437-108">En aktören service fjärrkommunikation lyssnare accepterar fjärråtkomst anrop tooactors och skickar dem tooa dispatcher tooroute toohello lämplig aktören instans.</span><span class="sxs-lookup"><span data-stu-id="a3437-108">An actor service remoting listener accepts remote access calls tooactors and sends them tooa dispatcher tooroute toohello appropriate actor instance.</span></span>
* <span data-ttu-id="a3437-109">hello aktören Tillståndsprovidern radbryts tillståndsprovidrar (till exempel hello tillförlitliga samlingar tillståndsprovidern) och ger ett nätverkskort för hantering av aktören tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a3437-109">hello Actor State Provider wraps state providers (such as hello Reliable Collections state provider) and provides an adapter for actor state management.</span></span>

<span data-ttu-id="a3437-110">Dessa komponenter som tillsammans bildar hello tillförlitliga aktören framework.</span><span class="sxs-lookup"><span data-stu-id="a3437-110">These components together form hello Reliable Actor framework.</span></span>

## <a name="service-layering"></a><span data-ttu-id="a3437-111">Tjänsten lager</span><span class="sxs-lookup"><span data-stu-id="a3437-111">Service layering</span></span>
<span data-ttu-id="a3437-112">Eftersom hello aktören själva tjänsten är en tillförlitlig tjänst, alla hello [programmodell](service-fabric-application-model.md), livscykel, [paketering](service-fabric-package-apps.md), [distribution](service-fabric-deploy-remove-applications.md), uppgradera och skalning begreppet Reliable Services gäller hello samma sätt tooactor tjänster.</span><span class="sxs-lookup"><span data-stu-id="a3437-112">Because hello actor service itself is a reliable service, all hello [application model](service-fabric-application-model.md), lifecycle, [packaging](service-fabric-package-apps.md), [deployment](service-fabric-deploy-remove-applications.md), upgrade, and scaling concepts of Reliable Services apply hello same way tooactor services.</span></span> 

![Aktören service lager][1]

<span data-ttu-id="a3437-114">hello Visar föregående diagram hello förhållandet mellan hello Service Fabric-programramverk och användarkod.</span><span class="sxs-lookup"><span data-stu-id="a3437-114">hello preceding diagram shows hello relationship between hello Service Fabric application frameworks and user code.</span></span> <span data-ttu-id="a3437-115">Blå element representerar hello Reliable Services application framework, orange representerar hello tillförlitliga aktören framework och grönt representerar användarkod.</span><span class="sxs-lookup"><span data-stu-id="a3437-115">Blue elements represent hello Reliable Services application framework, orange represents hello Reliable Actor framework, and green represents user code.</span></span>

<span data-ttu-id="a3437-116">I Reliable Services tjänsten ärver hello `StatefulService` klass.</span><span class="sxs-lookup"><span data-stu-id="a3437-116">In Reliable Services, your service inherits hello `StatefulService` class.</span></span> <span data-ttu-id="a3437-117">Den här klassen är härledd från `StatefulServiceBase` (eller `StatelessService` för tillståndslösa tjänster).</span><span class="sxs-lookup"><span data-stu-id="a3437-117">This class is itself derived from `StatefulServiceBase` (or `StatelessService` for stateless services).</span></span> <span data-ttu-id="a3437-118">I Reliable Actors använder du hello aktören service.</span><span class="sxs-lookup"><span data-stu-id="a3437-118">In Reliable Actors, you use hello actor service.</span></span> <span data-ttu-id="a3437-119">hello aktören service är en annan implementering av hello `StatefulServiceBase` klassen implementerar hello aktören mönstret där din aktörer kör.</span><span class="sxs-lookup"><span data-stu-id="a3437-119">hello actor service is a different implementation of hello `StatefulServiceBase` class that implements hello actor pattern where your actors run.</span></span> <span data-ttu-id="a3437-120">Eftersom hello aktören själva tjänsten är bara en implementering av `StatefulServiceBase`, du kan skriva en egen tjänst som härleds från `ActorService` och implementera servicenivåer funktioner hello samma sätt som när arv `StatefulService`, som:</span><span class="sxs-lookup"><span data-stu-id="a3437-120">Because hello actor service itself is just an implementation of `StatefulServiceBase`, you can write your own service that derives from `ActorService` and implement service-level features hello same way you would when inheriting `StatefulService`, such as:</span></span>

* <span data-ttu-id="a3437-121">Tjänsten säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="a3437-121">Service backup and restore.</span></span>
* <span data-ttu-id="a3437-122">Delade funktioner för alla aktörer, till exempel strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="a3437-122">Shared functionality for all actors, for example, a circuit breaker.</span></span>
* <span data-ttu-id="a3437-123">RPC-anrop på hello aktören själva tjänsten och på varje enskild aktören.</span><span class="sxs-lookup"><span data-stu-id="a3437-123">Remote procedure calls on hello actor service itself and on each individual actor.</span></span>

> [!NOTE]
> <span data-ttu-id="a3437-124">Tillståndskänsliga tjänster stöds inte i Java-/ Linux.</span><span class="sxs-lookup"><span data-stu-id="a3437-124">Stateful services are not currently supported in Java/Linux.</span></span>

### <a name="using-hello-actor-service"></a><span data-ttu-id="a3437-125">Hello aktören-tjänsten</span><span class="sxs-lookup"><span data-stu-id="a3437-125">Using hello actor service</span></span>
<span data-ttu-id="a3437-126">Aktören instanser har åtkomst toohello aktören de körs.</span><span class="sxs-lookup"><span data-stu-id="a3437-126">Actor instances have access toohello actor service in which they are running.</span></span> <span data-ttu-id="a3437-127">Via hello aktören service hämta aktören instanser programmässigt kontext för hello-tjänster.</span><span class="sxs-lookup"><span data-stu-id="a3437-127">Through hello actor service, actor instances can programmatically obtain hello service context.</span></span> <span data-ttu-id="a3437-128">hello service kontexten har hello partitions-ID, namn, programnamn och andra plattformsspecifika Service Fabric-information:</span><span class="sxs-lookup"><span data-stu-id="a3437-128">hello service context has hello partition ID, service name, application name, and other Service Fabric platform-specific information:</span></span>

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```


<span data-ttu-id="a3437-129">Hello aktören service måste ha registrerats med en typ i hello Service Fabric runtime som alla Reliable Services.</span><span class="sxs-lookup"><span data-stu-id="a3437-129">Like all Reliable Services, hello actor service must be registered with a service type in hello Service Fabric runtime.</span></span> <span data-ttu-id="a3437-130">För hello aktören tjänsten toorun aktören-instanser måste också aktören-typen registreras med hello aktören tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a3437-130">For hello actor service toorun your actor instances, your actor type must also be registered with hello actor service.</span></span> <span data-ttu-id="a3437-131">Hej `ActorRuntime` registreringsmetod utför detta arbete för aktörer.</span><span class="sxs-lookup"><span data-stu-id="a3437-131">hello `ActorRuntime` registration method performs this work for actors.</span></span> <span data-ttu-id="a3437-132">Du kan registrera aktörstyp i hello enklaste fallet och hello aktören tjänsten med standardinställningarna används implicit:</span><span class="sxs-lookup"><span data-stu-id="a3437-132">In hello simplest case, you can just register your actor type, and hello actor service with default settings will implicitly be used:</span></span>

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

<span data-ttu-id="a3437-133">Du kan också använda ett lambda-uttryck som tillhandahålls av hello metoden tooconstruct hello aktören Registreringstjänst för dig själv.</span><span class="sxs-lookup"><span data-stu-id="a3437-133">Alternatively, you can use a lambda provided by hello registration method tooconstruct hello actor service yourself.</span></span> <span data-ttu-id="a3437-134">Du kan konfigurera hello aktören service och uttryckligen skapa dina aktören instanser, där du kan mata in beroenden tooyour aktören via sin konstruktor:</span><span class="sxs-lookup"><span data-stu-id="a3437-134">You can then configure hello actor service and explicitly construct your actor instances, where you can inject dependencies tooyour actor through its constructor:</span></span>

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

### <a name="actor-service-methods"></a><span data-ttu-id="a3437-135">Aktören Webbtjänstmetoder</span><span class="sxs-lookup"><span data-stu-id="a3437-135">Actor service methods</span></span>
<span data-ttu-id="a3437-136">Hej aktören tjänsten implementerar `IActorService` (C#) eller `ActorService` (Java), som i sin tur implementerar `IService` (C#) eller `Service` (Java).</span><span class="sxs-lookup"><span data-stu-id="a3437-136">hello Actor service implements `IActorService` (C#) or `ActorService` (Java), which in turn implements `IService` (C#) or `Service` (Java).</span></span> <span data-ttu-id="a3437-137">Det är hello-gränssnitt som används av Reliable Services fjärrkommunikation, vilket gör att RPC-anrop på Webbtjänstmetoder.</span><span class="sxs-lookup"><span data-stu-id="a3437-137">This is hello interface used by Reliable Services remoting, which allows remote procedure calls on service methods.</span></span> <span data-ttu-id="a3437-138">Den innehåller servicenivå metoder som kan anropas via fjärranslutning via tjänsten fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="a3437-138">It contains service-level methods that can be called remotely via service remoting.</span></span>

#### <a name="enumerating-actors"></a><span data-ttu-id="a3437-139">Räknar upp aktörer</span><span class="sxs-lookup"><span data-stu-id="a3437-139">Enumerating actors</span></span>
<span data-ttu-id="a3437-140">hello kan aktören en klient tooenumerate metadata om hello aktörer som är värd för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a3437-140">hello actor service allows a client tooenumerate metadata about hello actors that hello service is hosting.</span></span> <span data-ttu-id="a3437-141">Eftersom hello aktören service är en partitionerad tillståndskänslig service, utförs uppräkningen per partition.</span><span class="sxs-lookup"><span data-stu-id="a3437-141">Because hello actor service is a partitioned stateful service, enumeration is performed per partition.</span></span> <span data-ttu-id="a3437-142">Eftersom varje partition kan innehålla många aktörer, returneras hello uppräkning som en uppsättning växlingsbara resultat.</span><span class="sxs-lookup"><span data-stu-id="a3437-142">Because each partition might contain many actors, hello enumeration is returned as a set of paged results.</span></span> <span data-ttu-id="a3437-143">hello sidor upprepas över tills alla sidor är skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="a3437-143">hello pages are looped over until all pages are read.</span></span> <span data-ttu-id="a3437-144">följande exempel visar hur hello toocreate en lista över alla aktiva aktörer i en partition av en aktören-tjänst:</span><span class="sxs-lookup"><span data-stu-id="a3437-144">hello following example shows how toocreate a list of all active actors in one partition of an actor service:</span></span>

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a><span data-ttu-id="a3437-145">Om du tar bort aktörer</span><span class="sxs-lookup"><span data-stu-id="a3437-145">Deleting actors</span></span>
<span data-ttu-id="a3437-146">hello aktören tjänsten innehåller också en funktion för att ta bort aktörer:</span><span class="sxs-lookup"><span data-stu-id="a3437-146">hello actor service also provides a function for deleting actors:</span></span>

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

<span data-ttu-id="a3437-147">Mer information om att radera aktörer och deras tillstånd finns hello [aktören livscykel dokumentationen](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="a3437-147">For more information on deleting actors and their state, see hello [actor lifecycle documentation](service-fabric-reliable-actors-lifecycle.md).</span></span>

### <a name="custom-actor-service"></a><span data-ttu-id="a3437-148">Anpassade aktören service</span><span class="sxs-lookup"><span data-stu-id="a3437-148">Custom actor service</span></span>
<span data-ttu-id="a3437-149">Med hjälp av hello aktören registrering lambda, kan du registrera dina egna anpassade aktören-tjänst som härleds från `ActorService` (C#) och `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="a3437-149">By using hello actor registration lambda, you can register your own custom actor service that derives from `ActorService` (C#) and `FabricActorService` (Java).</span></span> <span data-ttu-id="a3437-150">I den här anpassade aktören-tjänsten du kan implementera din egen servicenivå funktioner genom att skriva en service-klass som ärver `ActorService` (C#) eller `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="a3437-150">In this custom actor service, you can implement your own service-level functionality by writing a service class that inherits `ActorService` (C#) or `FabricActorService` (Java).</span></span> <span data-ttu-id="a3437-151">En anpassad aktören tjänst ärver alla hello aktören runtime funktioner från `ActorService` (C#) eller `FabricActorService` (Java) och kan vara används tooimplement egna Webbtjänstmetoder.</span><span class="sxs-lookup"><span data-stu-id="a3437-151">A custom actor service inherits all hello actor runtime functionality from `ActorService` (C#) or `FabricActorService` (Java) and can be used tooimplement your own service methods.</span></span>

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
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
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

#### <a name="implementing-actor-backup-and-restore"></a><span data-ttu-id="a3437-152">Implementera aktören säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="a3437-152">Implementing actor backup and restore</span></span>
 <span data-ttu-id="a3437-153">I följande exempel hello, hello anpassade aktören service Exponerar en metod tooback aktören data genom att utnyttja hello fjärrkommunikation lyssnare finns redan i `ActorService`:</span><span class="sxs-lookup"><span data-stu-id="a3437-153">In hello following example, hello custom actor service exposes a method tooback up actor data by taking advantage of hello remoting listener already present in `ActorService`:</span></span>

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store hello contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store hello contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```


<span data-ttu-id="a3437-154">I det här exemplet `IMyActorService` är ett kontrakt för fjärrkommunikation som implementerar `IService` (C#) och `Service` (Java) och sedan implementeras av `MyActorService`.</span><span class="sxs-lookup"><span data-stu-id="a3437-154">In this example, `IMyActorService` is a remoting contract that implements `IService` (C#) and `Service` (Java), and is then implemented by `MyActorService`.</span></span> <span data-ttu-id="a3437-155">Genom att lägga till avtalet remoting, metoder på `IMyActorService` är också tillgängliga tooa klienten genom att skapa en proxy för fjärrkommunikation via `ActorServiceProxy`:</span><span class="sxs-lookup"><span data-stu-id="a3437-155">By adding this remoting contract, methods on `IMyActorService` are now also available tooa client by creating a remoting proxy via `ActorServiceProxy`:</span></span>

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

## <a name="application-model"></a><span data-ttu-id="a3437-156">Programmodell</span><span class="sxs-lookup"><span data-stu-id="a3437-156">Application model</span></span>
<span data-ttu-id="a3437-157">Aktören tjänsterna är tillförlitliga, så hello programmodell är hello samma.</span><span class="sxs-lookup"><span data-stu-id="a3437-157">Actor services are Reliable Services, so hello application model is hello same.</span></span> <span data-ttu-id="a3437-158">Dock generera hello aktören framework verktyg några hello programfiler modell du.</span><span class="sxs-lookup"><span data-stu-id="a3437-158">However, hello actor framework build tools generate some of hello application model files for you.</span></span>

### <a name="service-manifest"></a><span data-ttu-id="a3437-159">Tjänstmanifestet</span><span class="sxs-lookup"><span data-stu-id="a3437-159">Service manifest</span></span>
<span data-ttu-id="a3437-160">hello aktören framework verktyg generera automatiskt hello innehållet i aktören tjänstens ServiceManifest.xml fil.</span><span class="sxs-lookup"><span data-stu-id="a3437-160">hello actor framework build tools automatically generate hello contents of your actor service's ServiceManifest.xml file.</span></span> <span data-ttu-id="a3437-161">Den här filen innehåller:</span><span class="sxs-lookup"><span data-stu-id="a3437-161">This file includes:</span></span>

* <span data-ttu-id="a3437-162">Aktören tjänsttyp.</span><span class="sxs-lookup"><span data-stu-id="a3437-162">Actor service type.</span></span> <span data-ttu-id="a3437-163">namn på hello genereras baserat på din aktören projektnamn.</span><span class="sxs-lookup"><span data-stu-id="a3437-163">hello type name is generated based on your actor's project name.</span></span> <span data-ttu-id="a3437-164">Baserat på hello beständiga attribut i din aktören anges hello HasPersistedState flaggan också i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="a3437-164">Based on hello persistence attribute on your actor, hello HasPersistedState flag is also set accordingly.</span></span>
* <span data-ttu-id="a3437-165">Kodpaketet.</span><span class="sxs-lookup"><span data-stu-id="a3437-165">Code package.</span></span>
* <span data-ttu-id="a3437-166">Konfigurationspaketet.</span><span class="sxs-lookup"><span data-stu-id="a3437-166">Config package.</span></span>
* <span data-ttu-id="a3437-167">Resurser och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="a3437-167">Resources and endpoints.</span></span>

### <a name="application-manifest"></a><span data-ttu-id="a3437-168">Applikationsmanifestet</span><span class="sxs-lookup"><span data-stu-id="a3437-168">Application manifest</span></span>
<span data-ttu-id="a3437-169">hello aktören framework verktyg skapa automatiskt en standard service definition för aktören tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a3437-169">hello actor framework build tools automatically create a default service definition for your actor service.</span></span> <span data-ttu-id="a3437-170">hello verktyg fylla hello standardegenskaperna för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="a3437-170">hello build tools populate hello default service properties:</span></span>

* <span data-ttu-id="a3437-171">Ange replikantalet bestäms av hello beständiga attribut i din aktören.</span><span class="sxs-lookup"><span data-stu-id="a3437-171">Replica set count is determined by hello persistence attribute on your actor.</span></span> <span data-ttu-id="a3437-172">Varje gång hello beständiga attribut på din aktören ändras, hello set replikantalet i tjänstdefinitionen för hello standard återställs därför.</span><span class="sxs-lookup"><span data-stu-id="a3437-172">Each time hello persistence attribute on your actor is changed, hello replica set count in hello default service definition is reset accordingly.</span></span>
* <span data-ttu-id="a3437-173">Partitionsschemat och intervallet ställs tooUniform Int64 med hello fullständig Int64 viktiga intervall.</span><span class="sxs-lookup"><span data-stu-id="a3437-173">Partition scheme and range are set tooUniform Int64 with hello full Int64 key range.</span></span>

## <a name="service-fabric-partition-concepts-for-actors"></a><span data-ttu-id="a3437-174">Service Fabric-partition begrepp för aktörer</span><span class="sxs-lookup"><span data-stu-id="a3437-174">Service Fabric partition concepts for actors</span></span>
<span data-ttu-id="a3437-175">Aktörstjänster är partitionerad tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="a3437-175">Actor services are partitioned stateful services.</span></span> <span data-ttu-id="a3437-176">Varje partition för en tjänst aktören innehåller en uppsättning aktörer.</span><span class="sxs-lookup"><span data-stu-id="a3437-176">Each partition of an actor service contains a set of actors.</span></span> <span data-ttu-id="a3437-177">Tjänsten partitioner distribueras automatiskt över flera noder i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="a3437-177">Service partitions are automatically distributed over multiple nodes in Service Fabric.</span></span> <span data-ttu-id="a3437-178">Aktören instanser distribueras som ett resultat.</span><span class="sxs-lookup"><span data-stu-id="a3437-178">Actor instances are distributed as a result.</span></span>

![Aktören partitionering och distribution][5]

<span data-ttu-id="a3437-180">Reliable Services kan skapas med annan partitionsscheman och partition nyckelintervall.</span><span class="sxs-lookup"><span data-stu-id="a3437-180">Reliable Services can be created with different partition schemes and partition key ranges.</span></span> <span data-ttu-id="a3437-181">hello aktören tjänsten använder hello Int64 partitioneringsschema med hello fullständig Int64 viktiga intervallet toomap aktörer toopartitions.</span><span class="sxs-lookup"><span data-stu-id="a3437-181">hello actor service uses hello Int64 partitioning scheme with hello full Int64 key range toomap actors toopartitions.</span></span>

### <a name="actor-id"></a><span data-ttu-id="a3437-182">Aktörs-ID</span><span class="sxs-lookup"><span data-stu-id="a3437-182">Actor ID</span></span>
<span data-ttu-id="a3437-183">Varje aktören som skapas i hello-tjänsten har ett unikt ID som är associerade med den representeras av hello `ActorId` klass.</span><span class="sxs-lookup"><span data-stu-id="a3437-183">Each actor that's created in hello service has a unique ID associated with it, represented by hello `ActorId` class.</span></span> <span data-ttu-id="a3437-184">`ActorId`är ett täckande ID-värde som kan användas för jämn fördelning av aktörer över hello service partitioner genom att generera slumpmässigt ID: N:</span><span class="sxs-lookup"><span data-stu-id="a3437-184">`ActorId` is an opaque ID value that can be used for uniform distribution of actors across hello service partitions by generating random IDs:</span></span>

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


<span data-ttu-id="a3437-185">Varje `ActorId` är hashformaterats tooan Int64.</span><span class="sxs-lookup"><span data-stu-id="a3437-185">Every `ActorId` is hashed tooan Int64.</span></span> <span data-ttu-id="a3437-186">Det är därför hello aktören tjänsten måste använda ett Int64 partitioneringsschema med hello fullständig Int64 viktiga intervall.</span><span class="sxs-lookup"><span data-stu-id="a3437-186">This is why hello actor service must use an Int64 partitioning scheme with hello full Int64 key range.</span></span> <span data-ttu-id="a3437-187">Dock anpassade ID-värden kan användas för en `ActorID`, inklusive GUID/UUID: er, strängar och Int64s.</span><span class="sxs-lookup"><span data-stu-id="a3437-187">However, custom ID values can be used for an `ActorID`, including GUIDs/UUIDs, strings, and Int64s.</span></span>

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

<span data-ttu-id="a3437-188">När du använder GUID/UUID: er och strängar värdena hello hashformaterats tooan Int64.</span><span class="sxs-lookup"><span data-stu-id="a3437-188">When you're using GUIDs/UUIDs and strings, hello values are hashed tooan Int64.</span></span> <span data-ttu-id="a3437-189">Men när du uttryckligen ger ett Int64 tooan `ActorId`, hello Int64 mappas direkt tooa partition utan ytterligare hash.</span><span class="sxs-lookup"><span data-stu-id="a3437-189">However, when you're explicitly providing an Int64 tooan `ActorId`, hello Int64 will map directly tooa partition without further hashing.</span></span> <span data-ttu-id="a3437-190">Du kan använda den här tekniken toocontrol som partition hello aktörer är placerade i.</span><span class="sxs-lookup"><span data-stu-id="a3437-190">You can use this technique toocontrol which partition hello actors are placed in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3437-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a3437-191">Next steps</span></span>
* [<span data-ttu-id="a3437-192">Aktören tillståndshantering</span><span class="sxs-lookup"><span data-stu-id="a3437-192">Actor state management</span></span>](service-fabric-reliable-actors-state-management.md)
* [<span data-ttu-id="a3437-193">Aktören livscykel och skräp samling</span><span class="sxs-lookup"><span data-stu-id="a3437-193">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="a3437-194">Aktörer API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="a3437-194">Actors API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="a3437-195">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="a3437-195">.NET sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="a3437-196">Java-exempelkod</span><span class="sxs-lookup"><span data-stu-id="a3437-196">Java sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
