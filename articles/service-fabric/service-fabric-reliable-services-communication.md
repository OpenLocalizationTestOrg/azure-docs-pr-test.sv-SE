---
title: "Tillförlitlig kommunikation översikt över | Microsoft Docs"
description: "Översikt över Reliable Services kommunikation modellen, inklusive ingående lyssnare på tjänster, åtgärda slutpunkter och kommunikation mellan tjänster."
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
ms.openlocfilehash: b418904f50b772c12bfcdbb95beb9312c8b9fb00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-reliable-services-communication-apis"></a><span data-ttu-id="c85e6-103">Hur du använder Reliable Services kommunikationen API: er</span><span class="sxs-lookup"><span data-stu-id="c85e6-103">How to use the Reliable Services communication APIs</span></span>
<span data-ttu-id="c85e6-104">Azure Service Fabric en plattform som är helt oberoende om kommunikation mellan tjänster.</span><span class="sxs-lookup"><span data-stu-id="c85e6-104">Azure Service Fabric as a platform is completely agnostic about communication between services.</span></span> <span data-ttu-id="c85e6-105">Alla protokoll och stackar är acceptabla, från UDP till HTTP.</span><span class="sxs-lookup"><span data-stu-id="c85e6-105">All protocols and stacks are acceptable, from UDP to HTTP.</span></span> <span data-ttu-id="c85e6-106">Är det upp till tjänsten utvecklaren kan välja hur tjänster ska kommunicera.</span><span class="sxs-lookup"><span data-stu-id="c85e6-106">It's up to the service developer to choose how services should communicate.</span></span> <span data-ttu-id="c85e6-107">Application framework Reliable Services tillhandahåller inbyggd kommunikation stackar samt API: er som du kan använda för att skapa anpassade kommunikationskomponenter.</span><span class="sxs-lookup"><span data-stu-id="c85e6-107">The Reliable Services application framework provides built-in communication stacks as well as APIs that you can use to build your custom communication components.</span></span>

## <a name="set-up-service-communication"></a><span data-ttu-id="c85e6-108">Konfigurera service-kommunikation</span><span class="sxs-lookup"><span data-stu-id="c85e6-108">Set up service communication</span></span>
<span data-ttu-id="c85e6-109">Reliable Services API används ett enkelt gränssnitt för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="c85e6-109">The Reliable Services API uses a simple interface for service communication.</span></span> <span data-ttu-id="c85e6-110">Om du vill öppna en slutpunkt för din tjänst helt enkelt implementera gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="c85e6-110">To open an endpoint for your service, simply implement this interface:</span></span>

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

<span data-ttu-id="c85e6-111">Du kan sedan lägga till din kommunikation lyssnare implementering returneras i en åsidosättning för service-baserad klass-metoden.</span><span class="sxs-lookup"><span data-stu-id="c85e6-111">You can then add your communication listener implementation by returning it in a service-based class method override.</span></span>

<span data-ttu-id="c85e6-112">För tillståndslösa tjänster:</span><span class="sxs-lookup"><span data-stu-id="c85e6-112">For stateless services:</span></span>

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

<span data-ttu-id="c85e6-113">För tillståndskänsliga tjänster:</span><span class="sxs-lookup"><span data-stu-id="c85e6-113">For stateful services:</span></span>

> [!NOTE]
> <span data-ttu-id="c85e6-114">Tillståndskänsliga reliable services stöds inte i Java ännu.</span><span class="sxs-lookup"><span data-stu-id="c85e6-114">Stateful reliable services are not supported in Java yet.</span></span>
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

<span data-ttu-id="c85e6-115">I båda fallen måste returnera en samling av avlyssning.</span><span class="sxs-lookup"><span data-stu-id="c85e6-115">In both cases, you return a collection of listeners.</span></span> <span data-ttu-id="c85e6-116">Detta gör tjänsten att lyssna på flera slutpunkter kan potentiellt med olika protokoll, med hjälp av flera lyssnare.</span><span class="sxs-lookup"><span data-stu-id="c85e6-116">This allows your service to listen on multiple endpoints, potentially using different protocols, by using multiple listeners.</span></span> <span data-ttu-id="c85e6-117">Du kan till exempel ha en HTTP-lyssnare och en separat WebSocket-lyssnare.</span><span class="sxs-lookup"><span data-stu-id="c85e6-117">For example, you may have an HTTP listener and a separate WebSocket listener.</span></span> <span data-ttu-id="c85e6-118">Varje lyssnare hämtar ett namn och den resulterande insamlingen av *name: adressen* par representeras som ett JSON-objekt när en klient begär lyssnande-adresser för en tjänstinstans eller en partition.</span><span class="sxs-lookup"><span data-stu-id="c85e6-118">Each listener gets a name, and the resulting collection of *name : address* pairs are represented as a JSON object when a client requests the listening addresses for a service instance or a partition.</span></span>

<span data-ttu-id="c85e6-119">Åsidosättningen som returnerar en mängd ServiceInstanceListeners i en tillståndslös tjänst.</span><span class="sxs-lookup"><span data-stu-id="c85e6-119">In a stateless service, the override returns a collection of ServiceInstanceListeners.</span></span> <span data-ttu-id="c85e6-120">En `ServiceInstanceListener` innehåller en funktion för att skapa en `ICommunicationListener(C#) / CommunicationListener(Java)` och ger den ett namn.</span><span class="sxs-lookup"><span data-stu-id="c85e6-120">A `ServiceInstanceListener` contains a function to create an `ICommunicationListener(C#) / CommunicationListener(Java)` and gives it a name.</span></span> <span data-ttu-id="c85e6-121">Åsidosättningen som returnerar en mängd ServiceReplicaListeners för tillståndskänsliga tjänster.</span><span class="sxs-lookup"><span data-stu-id="c85e6-121">For stateful services, the override returns a collection of ServiceReplicaListeners.</span></span> <span data-ttu-id="c85e6-122">Detta är skiljer sig från motparten tillståndslös, eftersom en `ServiceReplicaListener` har ett alternativ för att öppna en `ICommunicationListener` på sekundära repliker.</span><span class="sxs-lookup"><span data-stu-id="c85e6-122">This is slightly different from its stateless counterpart, because a `ServiceReplicaListener` has an option to open an `ICommunicationListener` on secondary replicas.</span></span> <span data-ttu-id="c85e6-123">Inte bara kan du använda flera kommunikationslyssnarna i en tjänst, men du kan också ange vilka lyssnare acceptera begäranden på sekundära repliker och vilka som bara lyssna på primära repliker.</span><span class="sxs-lookup"><span data-stu-id="c85e6-123">Not only can you use multiple communication listeners in a service, but you can also specify which listeners accept requests on secondary replicas and which ones listen only on primary replicas.</span></span>

<span data-ttu-id="c85e6-124">Till exempel kan du ha en ServiceRemotingListener som tar RPC-anrop endast på primära repliker och en andra, anpassade lyssnare som tar skrivskyddade begäranden på sekundära repliker över HTTP:</span><span class="sxs-lookup"><span data-stu-id="c85e6-124">For example, you can have a ServiceRemotingListener that takes RPC calls only on primary replicas, and a second, custom listener that takes read requests on secondary replicas over HTTP:</span></span>

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
> <span data-ttu-id="c85e6-125">När du skapar flera lyssnare för en tjänst, varje lyssnare **måste** ges ett unikt namn.</span><span class="sxs-lookup"><span data-stu-id="c85e6-125">When creating multiple listeners for a service, each listener **must** be given a unique name.</span></span>
>
>

<span data-ttu-id="c85e6-126">Slutligen beskrivs de slutpunkter som krävs för tjänsten i den [tjänstmanifestet](service-fabric-application-model.md) under avsnittet på slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="c85e6-126">Finally, describe the endpoints that are required for the service in the [service manifest](service-fabric-application-model.md) under the section on endpoints.</span></span>

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

<span data-ttu-id="c85e6-127">Kommunikation-lyssnaren kan komma åt slutpunkten resurserna för den från den `CodePackageActivationContext` i den `ServiceContext`.</span><span class="sxs-lookup"><span data-stu-id="c85e6-127">The communication listener can access the endpoint resources allocated to it from the `CodePackageActivationContext` in the `ServiceContext`.</span></span> <span data-ttu-id="c85e6-128">Lyssnaren kan sedan starta lyssna efter begäranden när den öppnas.</span><span class="sxs-lookup"><span data-stu-id="c85e6-128">The listener can then start listening for requests when it is opened.</span></span>

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> <span data-ttu-id="c85e6-129">Slutpunkten resurser som är gemensamma för hela tjänstepaketet och de tilldelas av Service Fabric när tjänstepaketet är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="c85e6-129">Endpoint resources are common to the entire service package, and they are allocated by Service Fabric when the service package is activated.</span></span> <span data-ttu-id="c85e6-130">Flera service repliker i samma ServiceHost kan dela samma port.</span><span class="sxs-lookup"><span data-stu-id="c85e6-130">Multiple service replicas hosted in the same ServiceHost may share the same port.</span></span> <span data-ttu-id="c85e6-131">Det innebär att kommunikation lyssnaren ska ha stöd för delning av port.</span><span class="sxs-lookup"><span data-stu-id="c85e6-131">This means that the communication listener should support port sharing.</span></span> <span data-ttu-id="c85e6-132">Det rekommenderade sättet att göra detta är för kommunikation lyssnaren att använda partitions-ID och repliken/instansen när den skapar lyssna-adress.</span><span class="sxs-lookup"><span data-stu-id="c85e6-132">The recommended way of doing this is for the communication listener to use the partition ID and replica/instance ID when it generates the listen address.</span></span>
>
>

### <a name="service-address-registration"></a><span data-ttu-id="c85e6-133">Adressen för tjänstregistrering</span><span class="sxs-lookup"><span data-stu-id="c85e6-133">Service address registration</span></span>
<span data-ttu-id="c85e6-134">En systemtjänst kallas den *namngivningstjänst* körs på Service Fabric-kluster.</span><span class="sxs-lookup"><span data-stu-id="c85e6-134">A system service called the *Naming Service* runs on Service Fabric clusters.</span></span> <span data-ttu-id="c85e6-135">Naming Service är en register för tjänster och de adresser som varje instans eller en replik av tjänsten lyssnar på.</span><span class="sxs-lookup"><span data-stu-id="c85e6-135">The Naming Service is a registrar for services and their addresses that each instance or replica of the service is listening on.</span></span> <span data-ttu-id="c85e6-136">När den `OpenAsync(C#) / openAsync(Java)` metod för ett `ICommunicationListener(C#) / CommunicationListener(Java)` har slutförts, dess returnera värdet hämtar registrerade i Naming Service.</span><span class="sxs-lookup"><span data-stu-id="c85e6-136">When the `OpenAsync(C#) / openAsync(Java)` method of an `ICommunicationListener(C#) / CommunicationListener(Java)` completes, its return value gets registered in the Naming Service.</span></span> <span data-ttu-id="c85e6-137">Värdet som publiceras i Naming Service är en sträng vars värde kan vara något alls.</span><span class="sxs-lookup"><span data-stu-id="c85e6-137">This return value that gets published in the Naming Service is a string whose value can be anything at all.</span></span> <span data-ttu-id="c85e6-138">Strängvärdet är klienter ser när de frågar efter en adress för tjänsten från Naming Service.</span><span class="sxs-lookup"><span data-stu-id="c85e6-138">This string value is what clients see when they ask for an address for the service from the Naming Service.</span></span>

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

    // the string returned here will be published in the Naming Service.
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

    /* the string returned here will be published in the Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

<span data-ttu-id="c85e6-139">Service Fabric innehåller API: er som gör att klienter och andra tjänster kan sedan ber för den här adressen av tjänstnamn.</span><span class="sxs-lookup"><span data-stu-id="c85e6-139">Service Fabric provides APIs that allow clients and other services to then ask for this address by service name.</span></span> <span data-ttu-id="c85e6-140">Detta är viktigt eftersom serviceadressen inte är statisk.</span><span class="sxs-lookup"><span data-stu-id="c85e6-140">This is important because the service address is not static.</span></span> <span data-ttu-id="c85e6-141">Tjänster flyttas i klustret för resursen belastningsutjämning och tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="c85e6-141">Services are moved around in the cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="c85e6-142">Det här är den mekanism som tillåter klienterna att lösa den lyssnande adressen för en tjänst.</span><span class="sxs-lookup"><span data-stu-id="c85e6-142">This is the mechanism that allow clients to resolve the listening address for a service.</span></span>

> [!NOTE]
> <span data-ttu-id="c85e6-143">En fullständig genomgång av hur du skriver en lyssnare för kommunikation finns [Service Fabric Web API-tjänster med OWIN värd själv](service-fabric-reliable-services-communication-webapi.md) för C#, men du kan skriva egna HTTP-serverimplementering för Java, se EchoServer program exempel på https://github.com/Azure-Samples/service-fabric-java-getting-started.</span><span class="sxs-lookup"><span data-stu-id="c85e6-143">For a complete walk-through of how to write a communication listener, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md) for C#, whereas for Java you can write your own HTTP server implementation, see EchoServer application example at https://github.com/Azure-Samples/service-fabric-java-getting-started.</span></span>
>
>

## <a name="communicating-with-a-service"></a><span data-ttu-id="c85e6-144">Kommunicerar med en tjänst</span><span class="sxs-lookup"><span data-stu-id="c85e6-144">Communicating with a service</span></span>
<span data-ttu-id="c85e6-145">API: et för tillförlitlig Services ger följande bibliotek om du vill skriva klienter som kommunicerar med tjänster.</span><span class="sxs-lookup"><span data-stu-id="c85e6-145">The Reliable Services API provides the following libraries to write clients that communicate with services.</span></span>

### <a name="service-endpoint-resolution"></a><span data-ttu-id="c85e6-146">Tjänsten slutpunktsmappning</span><span class="sxs-lookup"><span data-stu-id="c85e6-146">Service endpoint resolution</span></span>
<span data-ttu-id="c85e6-147">Det första steget att kommunikation med en tjänst är att lösa en slutpunktsadress av partition eller instans av tjänsten som du vill kommunicera.</span><span class="sxs-lookup"><span data-stu-id="c85e6-147">The first step to communication with a service is to resolve an endpoint address of the partition or instance of the service you want to talk to.</span></span> <span data-ttu-id="c85e6-148">Den `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` verktyget klassen är en grundläggande primitiv som hjälper klienterna att fastställa slutpunkten för en tjänst vid körning.</span><span class="sxs-lookup"><span data-stu-id="c85e6-148">The `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` utility class is a basic primitive that helps clients determine the endpoint of a service at runtime.</span></span> <span data-ttu-id="c85e6-149">Med Service Fabric-terminologi process för att fastställa slutpunkten för en tjänst kallas den *tjänsten slutpunktsmappning*.</span><span class="sxs-lookup"><span data-stu-id="c85e6-149">In Service Fabric terminology, the process of determining the endpoint of a service is referred to as the *service endpoint resolution*.</span></span>

<span data-ttu-id="c85e6-150">För att ansluta till tjänster i ett kluster, skapas ServicePartitionResolver med standardinställningar.</span><span class="sxs-lookup"><span data-stu-id="c85e6-150">To connect to services within a cluster, ServicePartitionResolver can be created using default settings.</span></span> <span data-ttu-id="c85e6-151">Detta är den rekommenderade användningen för de flesta situationer:</span><span class="sxs-lookup"><span data-stu-id="c85e6-151">This is the recommended usage for most situations:</span></span>

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

<span data-ttu-id="c85e6-152">En ServicePartitionResolver kan skapas med en uppsättning klusterslutpunkter för gateway för att ansluta till tjänster i ett annat kluster.</span><span class="sxs-lookup"><span data-stu-id="c85e6-152">To connect to services in a different cluster, a ServicePartitionResolver can be created with a set of cluster gateway endpoints.</span></span> <span data-ttu-id="c85e6-153">Observera att gateway-slutpunkter är bara olika slutpunkter för att ansluta till samma kluster.</span><span class="sxs-lookup"><span data-stu-id="c85e6-153">Note that gateway endpoints are just different endpoints for connecting to the same cluster.</span></span> <span data-ttu-id="c85e6-154">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c85e6-154">For example:</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

<span data-ttu-id="c85e6-155">Du kan också `ServicePartitionResolver` kan få en funktion för att skapa en `FabricClient` för intern användning:</span><span class="sxs-lookup"><span data-stu-id="c85e6-155">Alternatively, `ServicePartitionResolver` can be given a function for creating a `FabricClient` to use internally:</span></span>

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

<span data-ttu-id="c85e6-156">`FabricClient`är det objekt som används för att kommunicera med Service Fabric-klustret för olika hanteringsåtgärder på klustret.</span><span class="sxs-lookup"><span data-stu-id="c85e6-156">`FabricClient` is the object that is used to communicate with the Service Fabric cluster for various management operations on the cluster.</span></span> <span data-ttu-id="c85e6-157">Detta är användbart när du vill ha mer kontroll över hur en partition DNS-matchare för tjänsten samverkar med klustret.</span><span class="sxs-lookup"><span data-stu-id="c85e6-157">This is useful when you want more control over how a service partition resolver interacts with your cluster.</span></span> <span data-ttu-id="c85e6-158">`FabricClient`Utför cachelagring internt och är vanligtvis dyrt att skapa, så det är viktigt att återanvända `FabricClient` instanser så mycket som möjligt.</span><span class="sxs-lookup"><span data-stu-id="c85e6-158">`FabricClient` performs caching internally and is generally expensive to create, so it is important to reuse `FabricClient` instances as much as possible.</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

<span data-ttu-id="c85e6-159">En Lös metod används sedan för att hämta adressen till en tjänst eller en tjänst partition för partitionerade tjänster.</span><span class="sxs-lookup"><span data-stu-id="c85e6-159">A resolve method is then used to retrieve the address of a service or a service partition for partitioned services.</span></span>

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

<span data-ttu-id="c85e6-160">En tjänstadress kan lösas med enkelt en ServicePartitionResolver, men mer arbete krävs för att se till att matcha adressen kan användas på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="c85e6-160">A service address can be resolved easily using a ServicePartitionResolver, but more work is required to ensure the resolved address can be used correctly.</span></span> <span data-ttu-id="c85e6-161">Din klient behöver identifiera om anslutningsförsöket misslyckades på grund av ett tillfälligt fel och kan göras (t.ex. tjänsten flyttas eller inte tillgänglig för tillfället) eller ett permanent fel (t.ex. tjänsten har tagits bort eller den begärda resursen finns inte längre).</span><span class="sxs-lookup"><span data-stu-id="c85e6-161">Your client needs to detect whether the connection attempt failed because of a transient error and can be retried (e.g., service moved or is temporarily unavailable), or a permanent error (e.g., service was deleted or the requested resource no longer exists).</span></span> <span data-ttu-id="c85e6-162">En tjänstinstans eller repliker kan flytta runt från nod till noden när som helst av flera skäl.</span><span class="sxs-lookup"><span data-stu-id="c85e6-162">Service instances or replicas can move around from node to node at any time for multiple reasons.</span></span> <span data-ttu-id="c85e6-163">Tjänsten adressen matchas via ServicePartitionResolver kan vara inaktuella av den tid som din klientkod försöker ansluta.</span><span class="sxs-lookup"><span data-stu-id="c85e6-163">The service address resolved through ServicePartitionResolver may be stale by the time your client code attempts to connect.</span></span> <span data-ttu-id="c85e6-164">I så fall igen måste klienten matcha adressen igen.</span><span class="sxs-lookup"><span data-stu-id="c85e6-164">In that case again the client needs to re-resolve the address.</span></span> <span data-ttu-id="c85e6-165">Att tillhandahålla den tidigare `ResolvedServicePartition` innebär att matcharen måste försöka igen i stället för att hämta en cachelagrad adress.</span><span class="sxs-lookup"><span data-stu-id="c85e6-165">Providing the previous `ResolvedServicePartition` indicates that the resolver needs to try again rather than simply retrieve a cached address.</span></span>

<span data-ttu-id="c85e6-166">Normalt behöver klientkoden inte fungerar med ServicePartitionResolver direkt.</span><span class="sxs-lookup"><span data-stu-id="c85e6-166">Typically, the client code need not work with the ServicePartitionResolver directly.</span></span> <span data-ttu-id="c85e6-167">Det skapas och skickas till kommunikation klienten fabriker i Reliable Services API.</span><span class="sxs-lookup"><span data-stu-id="c85e6-167">It is created and passed on to communication client factories in the Reliable Services API.</span></span> <span data-ttu-id="c85e6-168">Produktionsanläggningarna använder matcharen internt för att generera ett objekt som kan användas för att kommunicera med tjänster.</span><span class="sxs-lookup"><span data-stu-id="c85e6-168">The factories use the resolver internally to generate a client object that can be used to communicate with services.</span></span>

### <a name="communication-clients-and-factories"></a><span data-ttu-id="c85e6-169">Klienter för kommunikation och fabriker</span><span class="sxs-lookup"><span data-stu-id="c85e6-169">Communication clients and factories</span></span>
<span data-ttu-id="c85e6-170">Kommunikation factory biblioteket implementerar ett typiskt försök mönster för hantering av fel som underlättar sprider på nytt anslutningar till löst slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="c85e6-170">The communication factory library implements a typical fault-handling retry pattern that makes retrying connections to resolved service endpoints easier.</span></span> <span data-ttu-id="c85e6-171">Fabriken biblioteket kan försök igen när du anger felhanterare.</span><span class="sxs-lookup"><span data-stu-id="c85e6-171">The factory library provides the retry mechanism while you provide the error handlers.</span></span>

<span data-ttu-id="c85e6-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`Definierar grundläggande gränssnitt som implementerats av en kommunikation klientfabrik som ger klienter som kan kommunicera med Service Fabric-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="c85e6-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)` defines the base interface implemented by a communication client factory that produces clients that can talk to a Service Fabric service.</span></span> <span data-ttu-id="c85e6-173">Implementeringen av CommunicationClientFactory beror på stacken för kommunikation som används av Service Fabric-tjänsten där klienten vill kommunicera.</span><span class="sxs-lookup"><span data-stu-id="c85e6-173">The implementation of the CommunicationClientFactory depends on the communication stack used by the Service Fabric service where the client wants to communicate.</span></span> <span data-ttu-id="c85e6-174">Reliable Services API ger en `CommunicationClientFactoryBase<TCommunicationClient>`.</span><span class="sxs-lookup"><span data-stu-id="c85e6-174">The Reliable Services API provides a `CommunicationClientFactoryBase<TCommunicationClient>`.</span></span> <span data-ttu-id="c85e6-175">Detta ger en grundläggande implementering av gränssnittet CommunicationClientFactory och utför uppgifter som är gemensamma för alla staplar för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="c85e6-175">This provides a base implementation of the CommunicationClientFactory interface and performs tasks that are common to all the communication stacks.</span></span> <span data-ttu-id="c85e6-176">(Dessa uppgifter omfattar med en ServicePartitionResolver för att avgöra tjänstslutpunkten).</span><span class="sxs-lookup"><span data-stu-id="c85e6-176">(These tasks include using a ServicePartitionResolver to determine the service endpoint).</span></span> <span data-ttu-id="c85e6-177">Klienter implementera vanligtvis den abstrakta klassen CommunicationClientFactoryBase för att hantera logik som är specifik för kommunikation stacken.</span><span class="sxs-lookup"><span data-stu-id="c85e6-177">Clients usually implement the abstract CommunicationClientFactoryBase class to handle logic that is specific to the communication stack.</span></span>

<span data-ttu-id="c85e6-178">Kommunikation klienten bara tar emot en adress och används för att ansluta till en tjänst.</span><span class="sxs-lookup"><span data-stu-id="c85e6-178">The communication client just receives an address and uses it to connect to a service.</span></span> <span data-ttu-id="c85e6-179">Klienten kan använda det protokoll som den vill.</span><span class="sxs-lookup"><span data-stu-id="c85e6-179">The client can use whatever protocol it wants.</span></span>

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

<span data-ttu-id="c85e6-180">Klient-fabrik är främst ansvarar för att skapa klienter för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="c85e6-180">The client factory is primarily responsible for creating communication clients.</span></span> <span data-ttu-id="c85e6-181">För klienter som inte har en beständig anslutning, till exempel en klient för HTTP-behöver fabriken bara skapa och returnera klienten.</span><span class="sxs-lookup"><span data-stu-id="c85e6-181">For clients that don't maintain a persistent connection, such as an HTTP client, the factory only needs to create and return the client.</span></span> <span data-ttu-id="c85e6-182">Andra protokoll som underhåller en beständig anslutning, till exempel vissa binära protokoll ska också verifieras av fabriken att avgöra om anslutningen behöver skapas på nytt.</span><span class="sxs-lookup"><span data-stu-id="c85e6-182">Other protocols that maintain a persistent connection, such as some binary protocols, should also be validated by the factory to determine whether the connection needs to be re-created.</span></span>  

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

<span data-ttu-id="c85e6-183">Slutligen ansvarar en undantagshanterare för att fastställa åtgärd att vidta när ett undantag inträffar.</span><span class="sxs-lookup"><span data-stu-id="c85e6-183">Finally, an exception handler is responsible for determining what action to take when an exception occurs.</span></span> <span data-ttu-id="c85e6-184">Undantag är indelade i **återförsökbart** och **icke återförsökbart**.</span><span class="sxs-lookup"><span data-stu-id="c85e6-184">Exceptions are categorized into **retryable** and **non retryable**.</span></span>

* <span data-ttu-id="c85e6-185">**Ej återförsökbart** undantag bara hämta igen tillbaka till anroparen.</span><span class="sxs-lookup"><span data-stu-id="c85e6-185">**Non retryable** exceptions simply get rethrown back to the caller.</span></span>
* <span data-ttu-id="c85e6-186">**återförsökbart** undantag kategoriseras i **tillfälligt** och **inte är tillfällig**.</span><span class="sxs-lookup"><span data-stu-id="c85e6-186">**retryable** exceptions are further categorized into **transient** and **non-transient**.</span></span>
  * <span data-ttu-id="c85e6-187">**Tillfälligt** undantag är de som bara kan göras utan att återlösa slutpunktsadress service.</span><span class="sxs-lookup"><span data-stu-id="c85e6-187">**Transient** exceptions are those that can simply be retried without re-resolving the service endpoint address.</span></span> <span data-ttu-id="c85e6-188">Dessa omfattar tillfälliga nätverksproblem eller tjänsten felsvar förutom de som indikerar att slutpunktsadressen som tjänsten inte finns.</span><span class="sxs-lookup"><span data-stu-id="c85e6-188">These will include transient network problems or service error responses other than those that indicate the service endpoint address does not exist.</span></span>
  * <span data-ttu-id="c85e6-189">**Icke-tillfälligt** undantag är de som kräver tjänsten slutpunktsadress lösas på nytt.</span><span class="sxs-lookup"><span data-stu-id="c85e6-189">**Non-transient** exceptions are those that require the service endpoint address to be re-resolved.</span></span> <span data-ttu-id="c85e6-190">Dessa inkluderar undantag som anger tjänstslutpunkten inte kunde nås, har som anger tjänsten flyttats till en annan nod.</span><span class="sxs-lookup"><span data-stu-id="c85e6-190">These include exceptions that indicate the service endpoint could not be reached, indicating the service has moved to a different node.</span></span>

<span data-ttu-id="c85e6-191">Den `TryHandleException` gör ett beslut om ett angivet undantag.</span><span class="sxs-lookup"><span data-stu-id="c85e6-191">The `TryHandleException` makes a decision about a given exception.</span></span> <span data-ttu-id="c85e6-192">Om den **inte vet** vilka beslut att göra om ett undantag som den ska returnera **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="c85e6-192">If it **does not know** what decisions to make about an exception, it should return **false**.</span></span> <span data-ttu-id="c85e6-193">Om den **känner** vilka beslut att fatta, det måste ange resultatet i enlighet med detta och returnera **SANT**.</span><span class="sxs-lookup"><span data-stu-id="c85e6-193">If it **does know** what decision to make, it should set the result accordingly and return **true**.</span></span>

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

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
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

        /* if exceptionInformation.getException() is unknown (let the next ExceptionHandler attempt to handle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a><span data-ttu-id="c85e6-194">Alltihop</span><span class="sxs-lookup"><span data-stu-id="c85e6-194">Putting it all together</span></span>
<span data-ttu-id="c85e6-195">Med en `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, och `IExceptionHandler(C#) / ExceptionHandler(Java)` uppbyggd kring ett kommunikationsprotokoll en `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` ska radbrytas samtidigt och ger hantering av fel och tjänsten partition adress upplösning cirkel runt dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="c85e6-195">With an `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, and `IExceptionHandler(C#) / ExceptionHandler(Java)` built around a communication protocol, a `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` wraps it all together and provides the fault-handling and service partition address resolution loop around these components.</span></span>

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
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
      /* Communicate with the service using the client.
       */
   });

```

## <a name="next-steps"></a><span data-ttu-id="c85e6-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c85e6-196">Next steps</span></span>
* <span data-ttu-id="c85e6-197">Se ett exempel på HTTP-kommunikation mellan tjänster i en [C#-exempelprojektet på GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) eller [Java exempelprojektet på GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span><span class="sxs-lookup"><span data-stu-id="c85e6-197">See an example of HTTP communication between services in a [C# sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) or [Java sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span></span>
* [<span data-ttu-id="c85e6-198">RPC-anrop med Reliable Services fjärrkommunikation</span><span class="sxs-lookup"><span data-stu-id="c85e6-198">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="c85e6-199">Webb-API som använder OWIN i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="c85e6-199">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="c85e6-200">WCF-kommunikation med hjälp av Reliable Services</span><span class="sxs-lookup"><span data-stu-id="c85e6-200">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
