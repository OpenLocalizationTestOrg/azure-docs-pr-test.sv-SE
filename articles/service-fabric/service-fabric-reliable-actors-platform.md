---
title: "Reliable Actors på Service Fabric | Microsoft Docs"
description: "Beskriver hur Reliable Actors ovanpå Reliable Services och använda funktioner i Service Fabric-plattformen."
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
ms.openlocfilehash: 0a12da52b6e74c721cd25f89e7cde3c07153a396
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a><span data-ttu-id="576eb-103">Hur Reliable Actors använder Service Fabric-plattformen</span><span class="sxs-lookup"><span data-stu-id="576eb-103">How Reliable Actors use the Service Fabric platform</span></span>
<span data-ttu-id="576eb-104">Den här artikeln förklarar hur Reliable Actors fungerar på Azure Service Fabric-plattformen.</span><span class="sxs-lookup"><span data-stu-id="576eb-104">This article explains how Reliable Actors work on the Azure Service Fabric platform.</span></span> <span data-ttu-id="576eb-105">Reliable Actors som körs i ett ramverk som är värd för en implementering av en tillståndskänslig tillförlitlig tjänst kallas den *aktören tjänsten*.</span><span class="sxs-lookup"><span data-stu-id="576eb-105">Reliable Actors run in a framework that is hosted in an implementation of a stateful reliable service called the *actor service*.</span></span> <span data-ttu-id="576eb-106">Tjänsten aktören innehåller alla komponenter som krävs för att hantera livscykeln och meddelandet sändning för din aktörer:</span><span class="sxs-lookup"><span data-stu-id="576eb-106">The actor service contains all the components necessary to manage the lifecycle and message dispatching for your actors:</span></span>

* <span data-ttu-id="576eb-107">Aktören Runtime hanterar livscykeln skräpinsamling, och tillämpar Enkeltrådig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="576eb-107">The Actor Runtime manages lifecycle, garbage collection, and enforces single-threaded access.</span></span>
* <span data-ttu-id="576eb-108">En aktören service fjärrkommunikation lyssnare accepterar fjärråtkomst anrop till aktörer och skickar dem till en dispatcher att dirigera till lämplig aktören-instans.</span><span class="sxs-lookup"><span data-stu-id="576eb-108">An actor service remoting listener accepts remote access calls to actors and sends them to a dispatcher to route to the appropriate actor instance.</span></span>
* <span data-ttu-id="576eb-109">Aktören Tillståndsprovidern radbryts tillståndsprovidrar (till exempel tillståndsprovidern tillförlitliga samlingar) och ger ett nätverkskort för hantering av aktören tillstånd.</span><span class="sxs-lookup"><span data-stu-id="576eb-109">The Actor State Provider wraps state providers (such as the Reliable Collections state provider) and provides an adapter for actor state management.</span></span>

<span data-ttu-id="576eb-110">De här komponenterna utgör tillsammans tillförlitliga aktören ramen.</span><span class="sxs-lookup"><span data-stu-id="576eb-110">These components together form the Reliable Actor framework.</span></span>

## <a name="service-layering"></a><span data-ttu-id="576eb-111">Tjänsten lager</span><span class="sxs-lookup"><span data-stu-id="576eb-111">Service layering</span></span>
<span data-ttu-id="576eb-112">Eftersom tjänsten aktören är en tillförlitlig tjänst alla de [programmodell](service-fabric-application-model.md), livscykel, [paketering](service-fabric-package-apps.md), [distribution](service-fabric-deploy-remove-applications.md), uppgradera och skalning begreppet Reliable Services tillämpas på samma sätt i aktörstjänster.</span><span class="sxs-lookup"><span data-stu-id="576eb-112">Because the actor service itself is a reliable service, all the [application model](service-fabric-application-model.md), lifecycle, [packaging](service-fabric-package-apps.md), [deployment](service-fabric-deploy-remove-applications.md), upgrade, and scaling concepts of Reliable Services apply the same way to actor services.</span></span> 

![Aktören service lager][1]

<span data-ttu-id="576eb-114">Föregående diagram visar relationen mellan Service Fabric-programramverk och användarkod.</span><span class="sxs-lookup"><span data-stu-id="576eb-114">The preceding diagram shows the relationship between the Service Fabric application frameworks and user code.</span></span> <span data-ttu-id="576eb-115">Blå element representerar Reliable Services application framework, orange representerar tillförlitliga aktören framework och grönt representerar användarkod.</span><span class="sxs-lookup"><span data-stu-id="576eb-115">Blue elements represent the Reliable Services application framework, orange represents the Reliable Actor framework, and green represents user code.</span></span>

<span data-ttu-id="576eb-116">I Reliable Services tjänsten ärver den `StatefulService` klass.</span><span class="sxs-lookup"><span data-stu-id="576eb-116">In Reliable Services, your service inherits the `StatefulService` class.</span></span> <span data-ttu-id="576eb-117">Den här klassen är härledd från `StatefulServiceBase` (eller `StatelessService` för tillståndslösa tjänster).</span><span class="sxs-lookup"><span data-stu-id="576eb-117">This class is itself derived from `StatefulServiceBase` (or `StatelessService` for stateless services).</span></span> <span data-ttu-id="576eb-118">I Reliable Actors använder du tjänsten aktören.</span><span class="sxs-lookup"><span data-stu-id="576eb-118">In Reliable Actors, you use the actor service.</span></span> <span data-ttu-id="576eb-119">Aktören-tjänsten är en annan implementering av den `StatefulServiceBase` klassen som implementerar aktören mönstret där din aktörer kör.</span><span class="sxs-lookup"><span data-stu-id="576eb-119">The actor service is a different implementation of the `StatefulServiceBase` class that implements the actor pattern where your actors run.</span></span> <span data-ttu-id="576eb-120">Eftersom tjänsten aktören är bara en implementering av `StatefulServiceBase`, du kan skriva en egen tjänst som härleds från `ActorService` och implementera servicenivå funktioner på samma sätt som vid arv `StatefulService`, som:</span><span class="sxs-lookup"><span data-stu-id="576eb-120">Because the actor service itself is just an implementation of `StatefulServiceBase`, you can write your own service that derives from `ActorService` and implement service-level features the same way you would when inheriting `StatefulService`, such as:</span></span>

* <span data-ttu-id="576eb-121">Tjänsten säkerhetskopiering och återställning.</span><span class="sxs-lookup"><span data-stu-id="576eb-121">Service backup and restore.</span></span>
* <span data-ttu-id="576eb-122">Delade funktioner för alla aktörer, till exempel strömbrytare.</span><span class="sxs-lookup"><span data-stu-id="576eb-122">Shared functionality for all actors, for example, a circuit breaker.</span></span>
* <span data-ttu-id="576eb-123">RPC-anrop på tjänsten aktören och på varje enskild aktören.</span><span class="sxs-lookup"><span data-stu-id="576eb-123">Remote procedure calls on the actor service itself and on each individual actor.</span></span>

> [!NOTE]
> <span data-ttu-id="576eb-124">Tillståndskänsliga tjänster stöds inte i Java-/ Linux.</span><span class="sxs-lookup"><span data-stu-id="576eb-124">Stateful services are not currently supported in Java/Linux.</span></span>

### <a name="using-the-actor-service"></a><span data-ttu-id="576eb-125">Med hjälp av tjänsten aktören</span><span class="sxs-lookup"><span data-stu-id="576eb-125">Using the actor service</span></span>
<span data-ttu-id="576eb-126">Aktören instanser har åtkomst till tjänsten aktören de körs.</span><span class="sxs-lookup"><span data-stu-id="576eb-126">Actor instances have access to the actor service in which they are running.</span></span> <span data-ttu-id="576eb-127">Via tjänsten aktören hämta aktören instanser programmässigt kontext för tjänster.</span><span class="sxs-lookup"><span data-stu-id="576eb-127">Through the actor service, actor instances can programmatically obtain the service context.</span></span> <span data-ttu-id="576eb-128">Tjänsten kontexten har partitions-ID, namn, programnamn och andra plattformsspecifika Service Fabric-information:</span><span class="sxs-lookup"><span data-stu-id="576eb-128">The service context has the partition ID, service name, application name, and other Service Fabric platform-specific information:</span></span>

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


<span data-ttu-id="576eb-129">Precis som alla Reliable Services måste tjänsten aktören registreras med en typ i Service Fabric-körningsmiljön.</span><span class="sxs-lookup"><span data-stu-id="576eb-129">Like all Reliable Services, the actor service must be registered with a service type in the Service Fabric runtime.</span></span> <span data-ttu-id="576eb-130">För tjänsten aktören att köra aktören-instanser måste aktören-typen registreras med tjänsten aktören.</span><span class="sxs-lookup"><span data-stu-id="576eb-130">For the actor service to run your actor instances, your actor type must also be registered with the actor service.</span></span> <span data-ttu-id="576eb-131">Den `ActorRuntime` registreringsmetod utför detta arbete för aktörer.</span><span class="sxs-lookup"><span data-stu-id="576eb-131">The `ActorRuntime` registration method performs this work for actors.</span></span> <span data-ttu-id="576eb-132">Du kan registrera aktörstyp i det enklaste fallet och aktören tjänsten med standardinställningarna används implicit:</span><span class="sxs-lookup"><span data-stu-id="576eb-132">In the simplest case, you can just register your actor type, and the actor service with default settings will implicitly be used:</span></span>

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

<span data-ttu-id="576eb-133">Du kan också använda ett lambda-uttryck som tillhandahålls av metoden för registrering för att konstruera tjänsten aktören själv.</span><span class="sxs-lookup"><span data-stu-id="576eb-133">Alternatively, you can use a lambda provided by the registration method to construct the actor service yourself.</span></span> <span data-ttu-id="576eb-134">Du kan konfigurera tjänsten aktören och uttryckligen skapa dina aktören instanser, där du kan mata in beroenden din aktören via sin konstruktor:</span><span class="sxs-lookup"><span data-stu-id="576eb-134">You can then configure the actor service and explicitly construct your actor instances, where you can inject dependencies to your actor through its constructor:</span></span>

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

### <a name="actor-service-methods"></a><span data-ttu-id="576eb-135">Aktören Webbtjänstmetoder</span><span class="sxs-lookup"><span data-stu-id="576eb-135">Actor service methods</span></span>
<span data-ttu-id="576eb-136">Aktören tjänsten implementerar `IActorService` (C#) eller `ActorService` (Java), som i sin tur implementerar `IService` (C#) eller `Service` (Java).</span><span class="sxs-lookup"><span data-stu-id="576eb-136">The Actor service implements `IActorService` (C#) or `ActorService` (Java), which in turn implements `IService` (C#) or `Service` (Java).</span></span> <span data-ttu-id="576eb-137">Detta är det gränssnitt som används av Reliable Services fjärrkommunikation, vilket gör att RPC-anrop på Webbtjänstmetoder.</span><span class="sxs-lookup"><span data-stu-id="576eb-137">This is the interface used by Reliable Services remoting, which allows remote procedure calls on service methods.</span></span> <span data-ttu-id="576eb-138">Den innehåller servicenivå metoder som kan anropas via fjärranslutning via tjänsten fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="576eb-138">It contains service-level methods that can be called remotely via service remoting.</span></span>

#### <a name="enumerating-actors"></a><span data-ttu-id="576eb-139">Räknar upp aktörer</span><span class="sxs-lookup"><span data-stu-id="576eb-139">Enumerating actors</span></span>
<span data-ttu-id="576eb-140">Tjänsten aktören kan en klient att räkna upp metadata om aktörer som är värd för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="576eb-140">The actor service allows a client to enumerate metadata about the actors that the service is hosting.</span></span> <span data-ttu-id="576eb-141">Eftersom tjänsten aktören är en partitionerad tillståndskänslig service, utförs uppräkningen per partition.</span><span class="sxs-lookup"><span data-stu-id="576eb-141">Because the actor service is a partitioned stateful service, enumeration is performed per partition.</span></span> <span data-ttu-id="576eb-142">Eftersom varje partition kan innehålla många aktörer, returneras uppräkningen som en uppsättning växlingsbara resultat.</span><span class="sxs-lookup"><span data-stu-id="576eb-142">Because each partition might contain many actors, the enumeration is returned as a set of paged results.</span></span> <span data-ttu-id="576eb-143">Sidorna upprepas över tills alla sidor är skrivskyddade.</span><span class="sxs-lookup"><span data-stu-id="576eb-143">The pages are looped over until all pages are read.</span></span> <span data-ttu-id="576eb-144">I följande exempel visas hur du skapar en lista över alla aktiva aktörer i en partition av aktören tjänster:</span><span class="sxs-lookup"><span data-stu-id="576eb-144">The following example shows how to create a list of all active actors in one partition of an actor service:</span></span>

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

#### <a name="deleting-actors"></a><span data-ttu-id="576eb-145">Om du tar bort aktörer</span><span class="sxs-lookup"><span data-stu-id="576eb-145">Deleting actors</span></span>
<span data-ttu-id="576eb-146">Tjänsten aktören innehåller också en funktion för att ta bort aktörer:</span><span class="sxs-lookup"><span data-stu-id="576eb-146">The actor service also provides a function for deleting actors:</span></span>

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

<span data-ttu-id="576eb-147">Mer information om att radera aktörer och deras tillstånd finns det [aktören livscykel dokumentationen](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="576eb-147">For more information on deleting actors and their state, see the [actor lifecycle documentation](service-fabric-reliable-actors-lifecycle.md).</span></span>

### <a name="custom-actor-service"></a><span data-ttu-id="576eb-148">Anpassade aktören service</span><span class="sxs-lookup"><span data-stu-id="576eb-148">Custom actor service</span></span>
<span data-ttu-id="576eb-149">Du kan registrera dina egna anpassade aktören-tjänst som härleds från med hjälp av aktören registrering lambda `ActorService` (C#) och `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="576eb-149">By using the actor registration lambda, you can register your own custom actor service that derives from `ActorService` (C#) and `FabricActorService` (Java).</span></span> <span data-ttu-id="576eb-150">I den här anpassade aktören-tjänsten du kan implementera din egen servicenivå funktioner genom att skriva en service-klass som ärver `ActorService` (C#) eller `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="576eb-150">In this custom actor service, you can implement your own service-level functionality by writing a service class that inherits `ActorService` (C#) or `FabricActorService` (Java).</span></span> <span data-ttu-id="576eb-151">En anpassad aktören tjänst ärver alla aktören runtime funktioner från `ActorService` (C#) eller `FabricActorService` (Java) och kan användas för att implementera din egen Webbtjänstmetoder.</span><span class="sxs-lookup"><span data-stu-id="576eb-151">A custom actor service inherits all the actor runtime functionality from `ActorService` (C#) or `FabricActorService` (Java) and can be used to implement your own service methods.</span></span>

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

#### <a name="implementing-actor-backup-and-restore"></a><span data-ttu-id="576eb-152">Implementera aktören säkerhetskopiering och återställning</span><span class="sxs-lookup"><span data-stu-id="576eb-152">Implementing actor backup and restore</span></span>
 <span data-ttu-id="576eb-153">I följande exempel anpassade aktören tjänsten Exponerar en metod för att säkerhetskopiera aktören data genom att utnyttja fjärrkommunikation lyssnaren finns redan i `ActorService`:</span><span class="sxs-lookup"><span data-stu-id="576eb-153">In the following example, the custom actor service exposes a method to back up actor data by taking advantage of the remoting listener already present in `ActorService`:</span></span>

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
           // store the contents of backupInfo.Directory
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
           // store the contents of backupInfo.Directory
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


<span data-ttu-id="576eb-154">I det här exemplet `IMyActorService` är ett kontrakt för fjärrkommunikation som implementerar `IService` (C#) och `Service` (Java) och sedan implementeras av `MyActorService`.</span><span class="sxs-lookup"><span data-stu-id="576eb-154">In this example, `IMyActorService` is a remoting contract that implements `IService` (C#) and `Service` (Java), and is then implemented by `MyActorService`.</span></span> <span data-ttu-id="576eb-155">Genom att lägga till avtalet remoting, metoder på `IMyActorService` är nu också tillgängliga för en klient genom att skapa en fjärrproxy via `ActorServiceProxy`:</span><span class="sxs-lookup"><span data-stu-id="576eb-155">By adding this remoting contract, methods on `IMyActorService` are now also available to a client by creating a remoting proxy via `ActorServiceProxy`:</span></span>

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

## <a name="application-model"></a><span data-ttu-id="576eb-156">Programmodell</span><span class="sxs-lookup"><span data-stu-id="576eb-156">Application model</span></span>
<span data-ttu-id="576eb-157">Aktören tjänsterna är tillförlitliga, så att programmet modellen är samma.</span><span class="sxs-lookup"><span data-stu-id="576eb-157">Actor services are Reliable Services, so the application model is the same.</span></span> <span data-ttu-id="576eb-158">Dock generera aktören framework build-verktyg några av modellen programfilerna för dig.</span><span class="sxs-lookup"><span data-stu-id="576eb-158">However, the actor framework build tools generate some of the application model files for you.</span></span>

### <a name="service-manifest"></a><span data-ttu-id="576eb-159">Tjänstmanifestet</span><span class="sxs-lookup"><span data-stu-id="576eb-159">Service manifest</span></span>
<span data-ttu-id="576eb-160">Aktören framework build-verktyg generera automatiskt innehållet i aktören tjänstens ServiceManifest.xml fil.</span><span class="sxs-lookup"><span data-stu-id="576eb-160">The actor framework build tools automatically generate the contents of your actor service's ServiceManifest.xml file.</span></span> <span data-ttu-id="576eb-161">Den här filen innehåller:</span><span class="sxs-lookup"><span data-stu-id="576eb-161">This file includes:</span></span>

* <span data-ttu-id="576eb-162">Aktören tjänsttyp.</span><span class="sxs-lookup"><span data-stu-id="576eb-162">Actor service type.</span></span> <span data-ttu-id="576eb-163">Typnamnet genereras baserat på din aktören projektnamn.</span><span class="sxs-lookup"><span data-stu-id="576eb-163">The type name is generated based on your actor's project name.</span></span> <span data-ttu-id="576eb-164">Baserat på attributet beständiga på din aktören ställs flaggan HasPersistedState också in därefter.</span><span class="sxs-lookup"><span data-stu-id="576eb-164">Based on the persistence attribute on your actor, the HasPersistedState flag is also set accordingly.</span></span>
* <span data-ttu-id="576eb-165">Kodpaketet.</span><span class="sxs-lookup"><span data-stu-id="576eb-165">Code package.</span></span>
* <span data-ttu-id="576eb-166">Konfigurationspaketet.</span><span class="sxs-lookup"><span data-stu-id="576eb-166">Config package.</span></span>
* <span data-ttu-id="576eb-167">Resurser och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="576eb-167">Resources and endpoints.</span></span>

### <a name="application-manifest"></a><span data-ttu-id="576eb-168">Applikationsmanifestet</span><span class="sxs-lookup"><span data-stu-id="576eb-168">Application manifest</span></span>
<span data-ttu-id="576eb-169">Aktören framework build-verktyg skapa automatiskt en standard service definition för aktören tjänsten.</span><span class="sxs-lookup"><span data-stu-id="576eb-169">The actor framework build tools automatically create a default service definition for your actor service.</span></span> <span data-ttu-id="576eb-170">Build-verktyg fylla standardegenskaperna för tjänsten:</span><span class="sxs-lookup"><span data-stu-id="576eb-170">The build tools populate the default service properties:</span></span>

* <span data-ttu-id="576eb-171">Ange replikantalet bestäms av beständiga attribut i din aktören.</span><span class="sxs-lookup"><span data-stu-id="576eb-171">Replica set count is determined by the persistence attribute on your actor.</span></span> <span data-ttu-id="576eb-172">Varje gång beständiga attribut i din aktören ändras återställs uppsättningen replikantalet i tjänstdefinitionen standard därför.</span><span class="sxs-lookup"><span data-stu-id="576eb-172">Each time the persistence attribute on your actor is changed, the replica set count in the default service definition is reset accordingly.</span></span>
* <span data-ttu-id="576eb-173">Partitionsschemat och intervallet är inställda Uniform Int64 med fullständig viktiga Int64-intervall.</span><span class="sxs-lookup"><span data-stu-id="576eb-173">Partition scheme and range are set to Uniform Int64 with the full Int64 key range.</span></span>

## <a name="service-fabric-partition-concepts-for-actors"></a><span data-ttu-id="576eb-174">Service Fabric-partition begrepp för aktörer</span><span class="sxs-lookup"><span data-stu-id="576eb-174">Service Fabric partition concepts for actors</span></span>
<span data-ttu-id="576eb-175">Aktörstjänster är partitionerad tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="576eb-175">Actor services are partitioned stateful services.</span></span> <span data-ttu-id="576eb-176">Varje partition för en tjänst aktören innehåller en uppsättning aktörer.</span><span class="sxs-lookup"><span data-stu-id="576eb-176">Each partition of an actor service contains a set of actors.</span></span> <span data-ttu-id="576eb-177">Tjänsten partitioner distribueras automatiskt över flera noder i Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="576eb-177">Service partitions are automatically distributed over multiple nodes in Service Fabric.</span></span> <span data-ttu-id="576eb-178">Aktören instanser distribueras som ett resultat.</span><span class="sxs-lookup"><span data-stu-id="576eb-178">Actor instances are distributed as a result.</span></span>

![Aktören partitionering och distribution][5]

<span data-ttu-id="576eb-180">Reliable Services kan skapas med annan partitionsscheman och partition nyckelintervall.</span><span class="sxs-lookup"><span data-stu-id="576eb-180">Reliable Services can be created with different partition schemes and partition key ranges.</span></span> <span data-ttu-id="576eb-181">Tjänsten aktören använder Int64 partitioneringsschema med fullständig viktiga Int64-intervall för att mappa aktörer till partitioner.</span><span class="sxs-lookup"><span data-stu-id="576eb-181">The actor service uses the Int64 partitioning scheme with the full Int64 key range to map actors to partitions.</span></span>

### <a name="actor-id"></a><span data-ttu-id="576eb-182">Aktörs-ID</span><span class="sxs-lookup"><span data-stu-id="576eb-182">Actor ID</span></span>
<span data-ttu-id="576eb-183">Varje aktören som skapas i tjänsten har ett unikt ID som är associerade med den representeras av den `ActorId` klass.</span><span class="sxs-lookup"><span data-stu-id="576eb-183">Each actor that's created in the service has a unique ID associated with it, represented by the `ActorId` class.</span></span> <span data-ttu-id="576eb-184">`ActorId`är ett täckande ID-värde som kan användas för jämn fördelning av aktörer över partitioner för tjänsten genom att generera slumpmässigt ID: N:</span><span class="sxs-lookup"><span data-stu-id="576eb-184">`ActorId` is an opaque ID value that can be used for uniform distribution of actors across the service partitions by generating random IDs:</span></span>

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


<span data-ttu-id="576eb-185">Varje `ActorId` hash-kodas till en Int64.</span><span class="sxs-lookup"><span data-stu-id="576eb-185">Every `ActorId` is hashed to an Int64.</span></span> <span data-ttu-id="576eb-186">Det är därför aktören tjänsten måste använda ett Int64 partitioneringsschema med fullständig viktiga Int64-intervall.</span><span class="sxs-lookup"><span data-stu-id="576eb-186">This is why the actor service must use an Int64 partitioning scheme with the full Int64 key range.</span></span> <span data-ttu-id="576eb-187">Dock anpassade ID-värden kan användas för en `ActorID`, inklusive GUID/UUID: er, strängar och Int64s.</span><span class="sxs-lookup"><span data-stu-id="576eb-187">However, custom ID values can be used for an `ActorID`, including GUIDs/UUIDs, strings, and Int64s.</span></span>

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

<span data-ttu-id="576eb-188">När du använder GUID/UUID: er och strängar värdena-kodas till en Int64.</span><span class="sxs-lookup"><span data-stu-id="576eb-188">When you're using GUIDs/UUIDs and strings, the values are hashed to an Int64.</span></span> <span data-ttu-id="576eb-189">Men när du uttryckligen ger Int64 till en `ActorId`, Int64 mappas direkt till en partition utan ytterligare hash.</span><span class="sxs-lookup"><span data-stu-id="576eb-189">However, when you're explicitly providing an Int64 to an `ActorId`, the Int64 will map directly to a partition without further hashing.</span></span> <span data-ttu-id="576eb-190">Du kan använda den här tekniken för att styra vilka partition aktörer placeras i.</span><span class="sxs-lookup"><span data-stu-id="576eb-190">You can use this technique to control which partition the actors are placed in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="576eb-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="576eb-191">Next steps</span></span>
* [<span data-ttu-id="576eb-192">Aktören tillståndshantering</span><span class="sxs-lookup"><span data-stu-id="576eb-192">Actor state management</span></span>](service-fabric-reliable-actors-state-management.md)
* [<span data-ttu-id="576eb-193">Aktören livscykel och skräp samling</span><span class="sxs-lookup"><span data-stu-id="576eb-193">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="576eb-194">Aktörer API-referensdokumentation</span><span class="sxs-lookup"><span data-stu-id="576eb-194">Actors API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="576eb-195">Exempelkod för .NET</span><span class="sxs-lookup"><span data-stu-id="576eb-195">.NET sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="576eb-196">Java-exempelkod</span><span class="sxs-lookup"><span data-stu-id="576eb-196">Java sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
