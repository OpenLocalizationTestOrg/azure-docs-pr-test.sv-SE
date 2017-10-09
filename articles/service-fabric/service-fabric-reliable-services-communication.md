---
title: "Översikt över aaaReliable Services kommunikation | Microsoft Docs"
description: "Översikt över hello Reliable Services kommunikation modellen, inklusive ingående lyssnare på tjänster, åtgärda slutpunkter och kommunikation mellan tjänster."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: 36217988-420e-409d-b0a4-e0e875b6eac8
ms.service: service-fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: 93a7017b50df0822969daa5ad78302c73e8ba641
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-reliable-services-communication-apis"></a><span data-ttu-id="8b502-103">Hur toouse hello Reliable Services kommunikation API: er</span><span class="sxs-lookup"><span data-stu-id="8b502-103">How toouse hello Reliable Services communication APIs</span></span>
<span data-ttu-id="8b502-104">Azure Service Fabric en plattform som är helt oberoende om kommunikation mellan tjänster.</span><span class="sxs-lookup"><span data-stu-id="8b502-104">Azure Service Fabric as a platform is completely agnostic about communication between services.</span></span> <span data-ttu-id="8b502-105">Alla protokoll och stackar är acceptabla, från UDP tooHTTP.</span><span class="sxs-lookup"><span data-stu-id="8b502-105">All protocols and stacks are acceptable, from UDP tooHTTP.</span></span> <span data-ttu-id="8b502-106">Är det upp toohello service developer toochoose hur tjänster ska kommunicera.</span><span class="sxs-lookup"><span data-stu-id="8b502-106">It's up toohello service developer toochoose how services should communicate.</span></span> <span data-ttu-id="8b502-107">hello Reliable Services-programmet innehåller inbyggd kommunikation staplar samt API: er som du kan använda toobuild komponenterna anpassade kommunikation.</span><span class="sxs-lookup"><span data-stu-id="8b502-107">hello Reliable Services application framework provides built-in communication stacks as well as APIs that you can use toobuild your custom communication components.</span></span>

## <a name="set-up-service-communication"></a><span data-ttu-id="8b502-108">Konfigurera service-kommunikation</span><span class="sxs-lookup"><span data-stu-id="8b502-108">Set up service communication</span></span>
<span data-ttu-id="8b502-109">hello Reliable Services API används ett enkelt gränssnitt för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="8b502-109">hello Reliable Services API uses a simple interface for service communication.</span></span> <span data-ttu-id="8b502-110">tooopen en slutpunkt för din tjänst helt enkelt implementera gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="8b502-110">tooopen an endpoint for your service, simply implement this interface:</span></span>

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

```java
public interface CommunicationListener {
    CompletableFuture<String> openAsync(CancellationToken cancellationToken);

    CompletableFuture<?> closeAsync(CancellationToken cancellationToken);

    void abort();
}
```

<span data-ttu-id="8b502-111">Du kan sedan lägga till din kommunikation lyssnare implementering returneras i en åsidosättning för service-baserad klass-metoden.</span><span class="sxs-lookup"><span data-stu-id="8b502-111">You can then add your communication listener implementation by returning it in a service-based class method override.</span></span>

<span data-ttu-id="8b502-112">För tillståndslösa tjänster:</span><span class="sxs-lookup"><span data-stu-id="8b502-112">For stateless services:</span></span>

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```
```java
public class MyStatelessService extends StatelessService {

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ...
    }
    ...
}
```

<span data-ttu-id="8b502-113">För tillståndskänsliga tjänster:</span><span class="sxs-lookup"><span data-stu-id="8b502-113">For stateful services:</span></span>

> [!NOTE]
> <span data-ttu-id="8b502-114">Tillståndskänsliga reliable services stöds inte i Java ännu.</span><span class="sxs-lookup"><span data-stu-id="8b502-114">Stateful reliable services are not supported in Java yet.</span></span>
>
>

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

<span data-ttu-id="8b502-115">I båda fallen måste returnera en samling av avlyssning.</span><span class="sxs-lookup"><span data-stu-id="8b502-115">In both cases, you return a collection of listeners.</span></span> <span data-ttu-id="8b502-116">Detta gör tjänsten-toolisten på flera slutpunkter kan potentiellt med olika protokoll, med hjälp av flera lyssnare.</span><span class="sxs-lookup"><span data-stu-id="8b502-116">This allows your service toolisten on multiple endpoints, potentially using different protocols, by using multiple listeners.</span></span> <span data-ttu-id="8b502-117">Du kan till exempel ha en HTTP-lyssnare och en separat WebSocket-lyssnare.</span><span class="sxs-lookup"><span data-stu-id="8b502-117">For example, you may have an HTTP listener and a separate WebSocket listener.</span></span> <span data-ttu-id="8b502-118">Varje lyssnare hämtar namn och hello resulterande samling *name: adressen* par representeras som ett JSON-objekt när en klient begär hello lyssnande adresser för en tjänstinstans eller en partition.</span><span class="sxs-lookup"><span data-stu-id="8b502-118">Each listener gets a name, and hello resulting collection of *name : address* pairs are represented as a JSON object when a client requests hello listening addresses for a service instance or a partition.</span></span>

<span data-ttu-id="8b502-119">I en tillståndslös tjänst returnerar hello åsidosättning en mängd ServiceInstanceListeners.</span><span class="sxs-lookup"><span data-stu-id="8b502-119">In a stateless service, hello override returns a collection of ServiceInstanceListeners.</span></span> <span data-ttu-id="8b502-120">En `ServiceInstanceListener` innehåller en funktion toocreate en `ICommunicationListener(C#) / CommunicationListener(Java)` och ger den ett namn.</span><span class="sxs-lookup"><span data-stu-id="8b502-120">A `ServiceInstanceListener` contains a function toocreate an `ICommunicationListener(C#) / CommunicationListener(Java)` and gives it a name.</span></span> <span data-ttu-id="8b502-121">Hello åsidosättning returnerar en mängd ServiceReplicaListeners för tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="8b502-121">For stateful services, hello override returns a collection of ServiceReplicaListeners.</span></span> <span data-ttu-id="8b502-122">Detta är skiljer sig från motparten tillståndslös, eftersom en `ServiceReplicaListener` har en alternativet tooopen en `ICommunicationListener` på sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="8b502-122">This is slightly different from its stateless counterpart, because a `ServiceReplicaListener` has an option tooopen an `ICommunicationListener` on secondary replicas.</span></span> <span data-ttu-id="8b502-123">Inte bara kan du använda flera kommunikationslyssnarna i en tjänst, men du kan också ange vilka lyssnare acceptera begäranden på sekundära repliker och vilka som bara lyssna på primära repliker.</span><span class="sxs-lookup"><span data-stu-id="8b502-123">Not only can you use multiple communication listeners in a service, but you can also specify which listeners accept requests on secondary replicas and which ones listen only on primary replicas.</span></span>

<span data-ttu-id="8b502-124">Till exempel kan du ha en ServiceRemotingListener som tar RPC-anrop endast på primära repliker och en andra, anpassade lyssnare som tar skrivskyddade begäranden på sekundära repliker över HTTP:</span><span class="sxs-lookup"><span data-stu-id="8b502-124">For example, you can have a ServiceRemotingListener that takes RPC calls only on primary replicas, and a second, custom listener that takes read requests on secondary replicas over HTTP:</span></span>

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [!NOTE]
> <span data-ttu-id="8b502-125">När du skapar flera lyssnare för en tjänst, varje lyssnare **måste** ges ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="8b502-125">When creating multiple listeners for a service, each listener **must** be given a unique name.</span></span>
>
>

<span data-ttu-id="8b502-126">Slutligen beskrivs hello-slutpunkter som krävs för hello i hello [tjänstmanifestet](service-fabric-application-model.md) under hello avsnitt på slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8b502-126">Finally, describe hello endpoints that are required for hello service in hello [service manifest](service-fabric-application-model.md) under hello section on endpoints.</span></span>

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

<span data-ttu-id="8b502-127">hello kommunikation lyssnaren kan komma åt hello endpoint anslås tooit från hello `CodePackageActivationContext` i hello `ServiceContext`.</span><span class="sxs-lookup"><span data-stu-id="8b502-127">hello communication listener can access hello endpoint resources allocated tooit from hello `CodePackageActivationContext` in hello `ServiceContext`.</span></span> <span data-ttu-id="8b502-128">hello-lyssnaren kan sedan börja lyssna efter begäranden när den öppnas.</span><span class="sxs-lookup"><span data-stu-id="8b502-128">hello listener can then start listening for requests when it is opened.</span></span>

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> <span data-ttu-id="8b502-129">Slutpunkten resurser är vanliga toohello hela tjänstepaketet och de tilldelas av Service Fabric när hello servicepaket har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="8b502-129">Endpoint resources are common toohello entire service package, and they are allocated by Service Fabric when hello service package is activated.</span></span> <span data-ttu-id="8b502-130">Flera service repliker i hello kan dela samma ServiceHost hello samma port.</span><span class="sxs-lookup"><span data-stu-id="8b502-130">Multiple service replicas hosted in hello same ServiceHost may share hello same port.</span></span> <span data-ttu-id="8b502-131">Det innebär att hello kommunikation lyssnare ska ha stöd för delning av port.</span><span class="sxs-lookup"><span data-stu-id="8b502-131">This means that hello communication listener should support port sharing.</span></span> <span data-ttu-id="8b502-132">hello bör kan du göra för hello kommunikation lyssnare toouse hello partitions-ID och repliken/instansen när den skapar hello lyssna adress.</span><span class="sxs-lookup"><span data-stu-id="8b502-132">hello recommended way of doing this is for hello communication listener toouse hello partition ID and replica/instance ID when it generates hello listen address.</span></span>
>
>

### <a name="service-address-registration"></a><span data-ttu-id="8b502-133">Adressen för tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="8b502-133">Service address registration</span></span>
<span data-ttu-id="8b502-134">En systemtjänst kallas hello *namngivningstjänst* körs på Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="8b502-134">A system service called hello *Naming Service* runs on Service Fabric clusters.</span></span> <span data-ttu-id="8b502-135">hello är Naming Service en register för tjänster och de adresser som varje instans eller en replik av hello tjänsten lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="8b502-135">hello Naming Service is a registrar for services and their addresses that each instance or replica of hello service is listening on.</span></span> <span data-ttu-id="8b502-136">När hello `OpenAsync(C#) / openAsync(Java)` metod för ett `ICommunicationListener(C#) / CommunicationListener(Java)` har slutförts, dess returnera värdet hämtar registrerade i hello Naming Service.</span><span class="sxs-lookup"><span data-stu-id="8b502-136">When hello `OpenAsync(C#) / openAsync(Java)` method of an `ICommunicationListener(C#) / CommunicationListener(Java)` completes, its return value gets registered in hello Naming Service.</span></span> <span data-ttu-id="8b502-137">Detta returvärde som hämtar publicerade i hello Naming Service är en sträng vars värde kan vara något alls.</span><span class="sxs-lookup"><span data-stu-id="8b502-137">This return value that gets published in hello Naming Service is a string whose value can be anything at all.</span></span> <span data-ttu-id="8b502-138">Strängvärdet är klienter ser när de frågar efter postadress hello från hello Naming Service.</span><span class="sxs-lookup"><span data-stu-id="8b502-138">This string value is what clients see when they ask for an address for hello service from hello Naming Service.</span></span>

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

    // hello string returned here will be published in hello Naming Service.
    return Task.FromResult(this.publishAddress);
}
```
```java
public CompletableFuture<String> openAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.getCodePackageActivationContext.getEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.getPort();

    this.publishAddress = String.format("http://%s:%d/", FabricRuntime.getNodeContext().getIpAddressOrFQDN(), port);

    this.webApp = new WebApp(port);
    this.webApp.start();

    /* hello string returned here will be published in hello Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

<span data-ttu-id="8b502-139">Service Fabric innehåller API: er som gör att klienter och andra tjänster toothen fråga för den här adressen av tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="8b502-139">Service Fabric provides APIs that allow clients and other services toothen ask for this address by service name.</span></span> <span data-ttu-id="8b502-140">Detta är viktigt eftersom hello-adress inte är statisk.</span><span class="sxs-lookup"><span data-stu-id="8b502-140">This is important because hello service address is not static.</span></span> <span data-ttu-id="8b502-141">Tjänster flyttas i hello kluster för resursen belastningsutjämning och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="8b502-141">Services are moved around in hello cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="8b502-142">Detta är hello mekanism som tillåter klienterna tooresolve hello lyssnar-adress för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="8b502-142">This is hello mechanism that allow clients tooresolve hello listening address for a service.</span></span>

> [!NOTE]
> <span data-ttu-id="8b502-143">För en fullständig genomgång av hur toowrite en lyssnare för kommunikation, se [Service Fabric Web API-tjänster med OWIN värd själv](service-fabric-reliable-services-communication-webapi.md) för C#, men du kan skriva egna HTTP-serverimplementering för Java, se EchoServer program exempel på https://github.com/Azure-Samples/service-fabric-java-getting-started.</span><span class="sxs-lookup"><span data-stu-id="8b502-143">For a complete walk-through of how toowrite a communication listener, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md) for C#, whereas for Java you can write your own HTTP server implementation, see EchoServer application example at https://github.com/Azure-Samples/service-fabric-java-getting-started.</span></span>
>
>

## <a name="communicating-with-a-service"></a><span data-ttu-id="8b502-144">Kommunicerar med en tjänst</span><span class="sxs-lookup"><span data-stu-id="8b502-144">Communicating with a service</span></span>
<span data-ttu-id="8b502-145">hello Reliable Services API ger hello följande bibliotek toowrite klienter som kommunicerar med tjänster.</span><span class="sxs-lookup"><span data-stu-id="8b502-145">hello Reliable Services API provides hello following libraries toowrite clients that communicate with services.</span></span>

### <a name="service-endpoint-resolution"></a><span data-ttu-id="8b502-146">Tjänsten slutpunktsmappning</span><span class="sxs-lookup"><span data-stu-id="8b502-146">Service endpoint resolution</span></span>
<span data-ttu-id="8b502-147">hello första steg toocommunication med en tjänst är tooresolve en slutpunktsadress av hello partition eller instans av hello-tjänster som du vill tootalk till.</span><span class="sxs-lookup"><span data-stu-id="8b502-147">hello first step toocommunication with a service is tooresolve an endpoint address of hello partition or instance of hello service you want tootalk to.</span></span> <span data-ttu-id="8b502-148">Hej `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` verktyget klassen är en grundläggande primitiv som hjälper klienterna att fastställa hello slutpunkten för en tjänst vid körning.</span><span class="sxs-lookup"><span data-stu-id="8b502-148">hello `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` utility class is a basic primitive that helps clients determine hello endpoint of a service at runtime.</span></span> <span data-ttu-id="8b502-149">Med Service Fabric-terminologi hello process för att fastställa hello slutpunkten för en tjänst är refererad tooas hello *tjänsten slutpunktsmappning*.</span><span class="sxs-lookup"><span data-stu-id="8b502-149">In Service Fabric terminology, hello process of determining hello endpoint of a service is referred tooas hello *service endpoint resolution*.</span></span>

<span data-ttu-id="8b502-150">tooconnect tooservices inom ett kluster, ServicePartitionResolver kan skapas med standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="8b502-150">tooconnect tooservices within a cluster, ServicePartitionResolver can be created using default settings.</span></span> <span data-ttu-id="8b502-151">Detta är hello rekommenderas användning för de flesta situationer:</span><span class="sxs-lookup"><span data-stu-id="8b502-151">This is hello recommended usage for most situations:</span></span>

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

<span data-ttu-id="8b502-152">tooconnect tooservices i ett annat kluster, en ServicePartitionResolver kan skapas med en uppsättning gateway klusterslutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8b502-152">tooconnect tooservices in a different cluster, a ServicePartitionResolver can be created with a set of cluster gateway endpoints.</span></span> <span data-ttu-id="8b502-153">Observera att gateway-slutpunkter är bara olika slutpunkter för att ansluta toohello samma kluster.</span><span class="sxs-lookup"><span data-stu-id="8b502-153">Note that gateway endpoints are just different endpoints for connecting toohello same cluster.</span></span> <span data-ttu-id="8b502-154">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8b502-154">For example:</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

<span data-ttu-id="8b502-155">Du kan också `ServicePartitionResolver` kan få en funktion för att skapa en `FabricClient` toouse internt:</span><span class="sxs-lookup"><span data-stu-id="8b502-155">Alternatively, `ServicePartitionResolver` can be given a function for creating a `FabricClient` toouse internally:</span></span>

```csharp
public delegate FabricClient CreateFabricClientDelegate();
```
```java
public FabricServicePartitionResolver(CreateFabricClient createFabricClient) {
...
}

public interface CreateFabricClient {
    public FabricClient getFabricClient();
}
```

<span data-ttu-id="8b502-156">`FabricClient`är hello-objekt som har använt toocommunicate med hello Service Fabric-klustret för olika hanteringsåtgärder på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8b502-156">`FabricClient` is hello object that is used toocommunicate with hello Service Fabric cluster for various management operations on hello cluster.</span></span> <span data-ttu-id="8b502-157">Detta är användbart när du vill ha mer kontroll över hur en partition DNS-matchare för tjänsten samverkar med klustret.</span><span class="sxs-lookup"><span data-stu-id="8b502-157">This is useful when you want more control over how a service partition resolver interacts with your cluster.</span></span> <span data-ttu-id="8b502-158">`FabricClient`Utför cachelagring internt och är vanligtvis dyra toocreate så att det är viktigt tooreuse `FabricClient` instanser så mycket som möjligt.</span><span class="sxs-lookup"><span data-stu-id="8b502-158">`FabricClient` performs caching internally and is generally expensive toocreate, so it is important tooreuse `FabricClient` instances as much as possible.</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

<span data-ttu-id="8b502-159">En Lös metod är och sedan använda tooretrieve hello-adressen för en tjänst eller en tjänst partition för partitionerade tjänster.</span><span class="sxs-lookup"><span data-stu-id="8b502-159">A resolve method is then used tooretrieve hello address of a service or a service partition for partitioned services.</span></span>

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();

CompletableFuture<ResolvedServicePartition> partition =
    resolver.resolveAsync(new URI("fabric:/MyApp/MyService"), new ServicePartitionKey());
```

<span data-ttu-id="8b502-160">En tjänstadress kan lösas med enkelt en ServicePartitionResolver, men det krävs mer arbete tooensure hello matcha adressen kan användas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="8b502-160">A service address can be resolved easily using a ServicePartitionResolver, but more work is required tooensure hello resolved address can be used correctly.</span></span> <span data-ttu-id="8b502-161">Klienten måste toodetect om hello anslutningsförsöket misslyckades på grund av ett tillfälligt fel och kan göras (t.ex. tjänsten flyttas eller inte tillgänglig för tillfället) eller ett permanent fel (t.ex. tjänsten har tagits bort eller hello begärda resursen finns inte längre).</span><span class="sxs-lookup"><span data-stu-id="8b502-161">Your client needs toodetect whether hello connection attempt failed because of a transient error and can be retried (e.g., service moved or is temporarily unavailable), or a permanent error (e.g., service was deleted or hello requested resource no longer exists).</span></span> <span data-ttu-id="8b502-162">En tjänstinstans eller repliker kan flytta från noden toonode när som helst av flera skäl.</span><span class="sxs-lookup"><span data-stu-id="8b502-162">Service instances or replicas can move around from node toonode at any time for multiple reasons.</span></span> <span data-ttu-id="8b502-163">hello-adress matchas via ServicePartitionResolver kan vara inaktuella hello gång klientkoden försöker tooconnect.</span><span class="sxs-lookup"><span data-stu-id="8b502-163">hello service address resolved through ServicePartitionResolver may be stale by hello time your client code attempts tooconnect.</span></span> <span data-ttu-id="8b502-164">I så fall igen hello klienten behöver toore Lös hello-adress.</span><span class="sxs-lookup"><span data-stu-id="8b502-164">In that case again hello client needs toore-resolve hello address.</span></span> <span data-ttu-id="8b502-165">Att tillhandahålla hello tidigare `ResolvedServicePartition` anger som hello lösare måste tootry igen i stället för att hämta en cachelagrad adress.</span><span class="sxs-lookup"><span data-stu-id="8b502-165">Providing hello previous `ResolvedServicePartition` indicates that hello resolver needs tootry again rather than simply retrieve a cached address.</span></span>

<span data-ttu-id="8b502-166">Normalt behöver hello klientkod inte fungerar med hello ServicePartitionResolver direkt.</span><span class="sxs-lookup"><span data-stu-id="8b502-166">Typically, hello client code need not work with hello ServicePartitionResolver directly.</span></span> <span data-ttu-id="8b502-167">Det skapas och skickas på toocommunication klienten fabriker i hello Reliable Services API.</span><span class="sxs-lookup"><span data-stu-id="8b502-167">It is created and passed on toocommunication client factories in hello Reliable Services API.</span></span> <span data-ttu-id="8b502-168">hello fabriker använder hello matcharen internt toogenerate ett objekt som kan använda toocommunicate med tjänster.</span><span class="sxs-lookup"><span data-stu-id="8b502-168">hello factories use hello resolver internally toogenerate a client object that can be used toocommunicate with services.</span></span>

### <a name="communication-clients-and-factories"></a><span data-ttu-id="8b502-169">Klienter för kommunikation och fabriker</span><span class="sxs-lookup"><span data-stu-id="8b502-169">Communication clients and factories</span></span>
<span data-ttu-id="8b502-170">hello kommunikation factory biblioteket implementerar ett typiskt försök mönster för hantering av fel som underlättar sprider på nytt anslutningar tooresolved slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="8b502-170">hello communication factory library implements a typical fault-handling retry pattern that makes retrying connections tooresolved service endpoints easier.</span></span> <span data-ttu-id="8b502-171">hello factory-bibliotek innehåller hello återförsöksmekanism medan du tillhandahåller hello felhanterare.</span><span class="sxs-lookup"><span data-stu-id="8b502-171">hello factory library provides hello retry mechanism while you provide hello error handlers.</span></span>

<span data-ttu-id="8b502-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`Definierar grundläggande hello gränssnitt som implementerats av en kommunikation klientfabrik som ger klienter som kan kommunicera tooa Service Fabric-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8b502-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)` defines hello base interface implemented by a communication client factory that produces clients that can talk tooa Service Fabric service.</span></span> <span data-ttu-id="8b502-173">Hej implementering av hello CommunicationClientFactory beror på hello kommunikation stack används hello Service Fabric-tjänsten där hello klient vill toocommunicate.</span><span class="sxs-lookup"><span data-stu-id="8b502-173">hello implementation of hello CommunicationClientFactory depends on hello communication stack used by hello Service Fabric service where hello client wants toocommunicate.</span></span> <span data-ttu-id="8b502-174">hello Reliable Services API ger en `CommunicationClientFactoryBase<TCommunicationClient>`.</span><span class="sxs-lookup"><span data-stu-id="8b502-174">hello Reliable Services API provides a `CommunicationClientFactoryBase<TCommunicationClient>`.</span></span> <span data-ttu-id="8b502-175">Detta ger en grundläggande implementering av hello CommunicationClientFactory gränssnitt och utför uppgifter som är vanliga tooall hello kommunikation stackar.</span><span class="sxs-lookup"><span data-stu-id="8b502-175">This provides a base implementation of hello CommunicationClientFactory interface and performs tasks that are common tooall hello communication stacks.</span></span> <span data-ttu-id="8b502-176">(Dessa uppgifter är med hjälp av en tjänstslutpunkt ServicePartitionResolver toodetermine hello).</span><span class="sxs-lookup"><span data-stu-id="8b502-176">(These tasks include using a ServicePartitionResolver toodetermine hello service endpoint).</span></span> <span data-ttu-id="8b502-177">Klienter implementera vanligtvis hello abstrakt CommunicationClientFactoryBase klassen toohandle logiken som är specifika toohello kommunikation stacken.</span><span class="sxs-lookup"><span data-stu-id="8b502-177">Clients usually implement hello abstract CommunicationClientFactoryBase class toohandle logic that is specific toohello communication stack.</span></span>

<span data-ttu-id="8b502-178">hello kommunikation klienten bara tar emot en adress och använder den tooconnect tooa service.</span><span class="sxs-lookup"><span data-stu-id="8b502-178">hello communication client just receives an address and uses it tooconnect tooa service.</span></span> <span data-ttu-id="8b502-179">hello-klienten kan använda det protokoll som den vill.</span><span class="sxs-lookup"><span data-stu-id="8b502-179">hello client can use whatever protocol it wants.</span></span>

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```
```java
public class MyCommunicationClient implements CommunicationClient {

    private ResolvedServicePartition resolvedServicePartition;
    private String listenerName;
    private ResolvedServiceEndpoint endPoint;

    /*
     * Getters and Setters
     */
}
```

<span data-ttu-id="8b502-180">Hej klientfabrik är främst ansvarar för att skapa klienter för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="8b502-180">hello client factory is primarily responsible for creating communication clients.</span></span> <span data-ttu-id="8b502-181">För klienter som inte har en beständig anslutning, till exempel en HTTP-klienten behöver bara hello factory toocreate och returnera hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="8b502-181">For clients that don't maintain a persistent connection, such as an HTTP client, hello factory only needs toocreate and return hello client.</span></span> <span data-ttu-id="8b502-182">Andra protokoll som underhåller en beständig anslutning, till exempel vissa binära protokoll ska också verifieras av hello factory toodetermine om hello anslutningen behöver toobe skapas på nytt.</span><span class="sxs-lookup"><span data-stu-id="8b502-182">Other protocols that maintain a persistent connection, such as some binary protocols, should also be validated by hello factory toodetermine whether hello connection needs toobe re-created.</span></span>  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```
```java
public class MyCommunicationClientFactory extends CommunicationClientFactoryBase<MyCommunicationClient> {

    @Override
    protected boolean validateClient(MyCommunicationClient clientChannel) {
    }

    @Override
    protected boolean validateClient(String endpoint, MyCommunicationClient client) {
    }

    @Override
    protected CompletableFuture<MyCommunicationClient> createClientAsync(String endpoint) {
    }

    @Override
    protected void abortClient(MyCommunicationClient client) {
    }
}
```

<span data-ttu-id="8b502-183">Slutligen ansvarar en undantagshanterare för att fastställa vilken åtgärd tootake när ett undantag inträffar.</span><span class="sxs-lookup"><span data-stu-id="8b502-183">Finally, an exception handler is responsible for determining what action tootake when an exception occurs.</span></span> <span data-ttu-id="8b502-184">Undantag är indelade i **återförsökbart** och **icke återförsökbart**.</span><span class="sxs-lookup"><span data-stu-id="8b502-184">Exceptions are categorized into **retryable** and **non retryable**.</span></span>

* <span data-ttu-id="8b502-185">**Ej återförsökbart** undantag bara hämta igen tillbaka toohello anroparen.</span><span class="sxs-lookup"><span data-stu-id="8b502-185">**Non retryable** exceptions simply get rethrown back toohello caller.</span></span>
* <span data-ttu-id="8b502-186">**återförsökbart** undantag kategoriseras i **tillfälligt** och **inte är tillfällig**.</span><span class="sxs-lookup"><span data-stu-id="8b502-186">**retryable** exceptions are further categorized into **transient** and **non-transient**.</span></span>
  * <span data-ttu-id="8b502-187">**Tillfälligt** undantag är de som bara kan göras utan att återlösa hello slutpunktsadress för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8b502-187">**Transient** exceptions are those that can simply be retried without re-resolving hello service endpoint address.</span></span> <span data-ttu-id="8b502-188">Dessa omfattar tillfälliga nätverksproblem eller tjänsten felsvar förutom de som visar hello slutpunktsadress för tjänsten finns inte.</span><span class="sxs-lookup"><span data-stu-id="8b502-188">These will include transient network problems or service error responses other than those that indicate hello service endpoint address does not exist.</span></span>
  * <span data-ttu-id="8b502-189">**Icke-tillfälligt** undantag är de som kräver hello endpoint adress toobe löst igen.</span><span class="sxs-lookup"><span data-stu-id="8b502-189">**Non-transient** exceptions are those that require hello service endpoint address toobe re-resolved.</span></span> <span data-ttu-id="8b502-190">Dessa inkluderar undantag som visar hello tjänstslutpunkten inte kunde nås, som visar hello-tjänsten har flyttat tooa annan nod.</span><span class="sxs-lookup"><span data-stu-id="8b502-190">These include exceptions that indicate hello service endpoint could not be reached, indicating hello service has moved tooa different node.</span></span>

<span data-ttu-id="8b502-191">Hej `TryHandleException` gör ett beslut om ett angivet undantag.</span><span class="sxs-lookup"><span data-stu-id="8b502-191">hello `TryHandleException` makes a decision about a given exception.</span></span> <span data-ttu-id="8b502-192">Om den **inte vet** vilka beslut toomake om ett undantag som den ska returnera **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="8b502-192">If it **does not know** what decisions toomake about an exception, it should return **false**.</span></span> <span data-ttu-id="8b502-193">Om den **känner** vilka beslut toomake som den ska ange hello resultatet i enlighet med detta och returnera **SANT**.</span><span class="sxs-lookup"><span data-stu-id="8b502-193">If it **does know** what decision toomake, it should set hello result accordingly and return **true**.</span></span>

```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let hello next IExceptionHandler attempt toohandle it)
        result = null;
        return false;
    }
}
```
```java
public class MyExceptionHandler implements ExceptionHandler {

    @Override
    public ExceptionHandlingResult handleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings) {        

        /* if exceptionInformation.getException() is known and is transient (can be retried without re-resolving)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), true, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;


        /* if exceptionInformation.getException() is known and is not transient (indicates a new service endpoint address must be resolved)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), false, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;

        /* if exceptionInformation.getException() is unknown (let hello next ExceptionHandler attempt toohandle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a><span data-ttu-id="8b502-194">Alltihop</span><span class="sxs-lookup"><span data-stu-id="8b502-194">Putting it all together</span></span>
<span data-ttu-id="8b502-195">Med en `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, och `IExceptionHandler(C#) / ExceptionHandler(Java)` uppbyggd kring ett kommunikationsprotokoll en `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` ska radbrytas samtidigt och ger hello fault-hantering och tjänsten partition adress upplösning loop runt dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="8b502-195">With an `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, and `IExceptionHandler(C#) / ExceptionHandler(Java)` built around a communication protocol, a `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` wraps it all together and provides hello fault-handling and service partition address resolution loop around these components.</span></span>

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with hello service using hello client.
   },
   CancellationToken.None);

```
```java
private MyCommunicationClientFactory myCommunicationClientFactory;
private URI myServiceUri;

FabricServicePartitionClient myServicePartitionClient = new FabricServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

CompletableFuture<?> result = myServicePartitionClient.invokeWithRetryAsync(client -> {
      /* Communicate with hello service using hello client.
       */
   });

```

## <a name="next-steps"></a><span data-ttu-id="8b502-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8b502-196">Next steps</span></span>
* <span data-ttu-id="8b502-197">Se ett exempel på HTTP-kommunikation mellan tjänster i en [C#-exempelprojektet på GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) eller [Java exempelprojektet på GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span><span class="sxs-lookup"><span data-stu-id="8b502-197">See an example of HTTP communication between services in a [C# sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) or [Java sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span></span>
* [<span data-ttu-id="8b502-198">RPC-anrop med Reliable Services fjärrkommunikation</span><span class="sxs-lookup"><span data-stu-id="8b502-198">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="8b502-199">Webb-API som använder OWIN i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="8b502-199">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="8b502-200">WCF-kommunikation med hjälp av Reliable Services</span><span class="sxs-lookup"><span data-stu-id="8b502-200">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
