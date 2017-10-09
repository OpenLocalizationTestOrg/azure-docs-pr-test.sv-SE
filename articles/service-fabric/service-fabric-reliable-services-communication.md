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
# <a name="how-toouse-hello-reliable-services-communication-apis"></a>Hur toouse hello Reliable Services kommunikation API: er
Azure Service Fabric en plattform som är helt oberoende om kommunikation mellan tjänster. Alla protokoll och stackar är acceptabla, från UDP tooHTTP. Är det upp toohello service developer toochoose hur tjänster ska kommunicera. hello Reliable Services-programmet innehåller inbyggd kommunikation staplar samt API: er som du kan använda toobuild komponenterna anpassade kommunikation.

## <a name="set-up-service-communication"></a>Konfigurera service-kommunikation
hello Reliable Services API används ett enkelt gränssnitt för kommunikation. tooopen en slutpunkt för din tjänst helt enkelt implementera gränssnittet:

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

Du kan sedan lägga till din kommunikation lyssnare implementering returneras i en åsidosättning för service-baserad klass-metoden.

För tillståndslösa tjänster:

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

För tillståndskänsliga tjänster:

> [!NOTE]
> Tillståndskänsliga reliable services stöds inte i Java ännu.
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

I båda fallen måste returnera en samling av avlyssning. Detta gör tjänsten-toolisten på flera slutpunkter kan potentiellt med olika protokoll, med hjälp av flera lyssnare. Du kan till exempel ha en HTTP-lyssnare och en separat WebSocket-lyssnare. Varje lyssnare hämtar namn och hello resulterande samling *name: adressen* par representeras som ett JSON-objekt när en klient begär hello lyssnande adresser för en tjänstinstans eller en partition.

I en tillståndslös tjänst returnerar hello åsidosättning en mängd ServiceInstanceListeners. En `ServiceInstanceListener` innehåller en funktion toocreate en `ICommunicationListener(C#) / CommunicationListener(Java)` och ger den ett namn. Hello åsidosättning returnerar en mängd ServiceReplicaListeners för tillståndskänsliga tjänster. Detta är skiljer sig från motparten tillståndslös, eftersom en `ServiceReplicaListener` har en alternativet tooopen en `ICommunicationListener` på sekundära repliker. Inte bara kan du använda flera kommunikationslyssnarna i en tjänst, men du kan också ange vilka lyssnare acceptera begäranden på sekundära repliker och vilka som bara lyssna på primära repliker.

Till exempel kan du ha en ServiceRemotingListener som tar RPC-anrop endast på primära repliker och en andra, anpassade lyssnare som tar skrivskyddade begäranden på sekundära repliker över HTTP:

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
> När du skapar flera lyssnare för en tjänst, varje lyssnare **måste** ges ett unikt namn.
>
>

Slutligen beskrivs hello-slutpunkter som krävs för hello i hello [tjänstmanifestet](service-fabric-application-model.md) under hello avsnitt på slutpunkter.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

hello kommunikation lyssnaren kan komma åt hello endpoint anslås tooit från hello `CodePackageActivationContext` i hello `ServiceContext`. hello-lyssnaren kan sedan börja lyssna efter begäranden när den öppnas.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> Slutpunkten resurser är vanliga toohello hela tjänstepaketet och de tilldelas av Service Fabric när hello servicepaket har aktiverats. Flera service repliker i hello kan dela samma ServiceHost hello samma port. Det innebär att hello kommunikation lyssnare ska ha stöd för delning av port. hello bör kan du göra för hello kommunikation lyssnare toouse hello partitions-ID och repliken/instansen när den skapar hello lyssna adress.
>
>

### <a name="service-address-registration"></a>Adressen för tjänstregistrering
En systemtjänst kallas hello *namngivningstjänst* körs på Service Fabric-kluster. hello är Naming Service en register för tjänster och de adresser som varje instans eller en replik av hello tjänsten lyssnar på. När hello `OpenAsync(C#) / openAsync(Java)` metod för ett `ICommunicationListener(C#) / CommunicationListener(Java)` har slutförts, dess returnera värdet hämtar registrerade i hello Naming Service. Detta returvärde som hämtar publicerade i hello Naming Service är en sträng vars värde kan vara något alls. Strängvärdet är klienter ser när de frågar efter postadress hello från hello Naming Service.

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

Service Fabric innehåller API: er som gör att klienter och andra tjänster toothen fråga för den här adressen av tjänstnamn. Detta är viktigt eftersom hello-adress inte är statisk. Tjänster flyttas i hello kluster för resursen belastningsutjämning och tillgänglighet. Detta är hello mekanism som tillåter klienterna tooresolve hello lyssnar-adress för en tjänst.

> [!NOTE]
> För en fullständig genomgång av hur toowrite en lyssnare för kommunikation, se [Service Fabric Web API-tjänster med OWIN värd själv](service-fabric-reliable-services-communication-webapi.md) för C#, men du kan skriva egna HTTP-serverimplementering för Java, se EchoServer program exempel på https://github.com/Azure-Samples/service-fabric-java-getting-started.
>
>

## <a name="communicating-with-a-service"></a>Kommunicerar med en tjänst
hello Reliable Services API ger hello följande bibliotek toowrite klienter som kommunicerar med tjänster.

### <a name="service-endpoint-resolution"></a>Tjänsten slutpunktsmappning
hello första steg toocommunication med en tjänst är tooresolve en slutpunktsadress av hello partition eller instans av hello-tjänster som du vill tootalk till. Hej `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` verktyget klassen är en grundläggande primitiv som hjälper klienterna att fastställa hello slutpunkten för en tjänst vid körning. Med Service Fabric-terminologi hello process för att fastställa hello slutpunkten för en tjänst är refererad tooas hello *tjänsten slutpunktsmappning*.

tooconnect tooservices inom ett kluster, ServicePartitionResolver kan skapas med standardinställningar. Detta är hello rekommenderas användning för de flesta situationer:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

tooconnect tooservices i ett annat kluster, en ServicePartitionResolver kan skapas med en uppsättning gateway klusterslutpunkter. Observera att gateway-slutpunkter är bara olika slutpunkter för att ansluta toohello samma kluster. Exempel:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Du kan också `ServicePartitionResolver` kan få en funktion för att skapa en `FabricClient` toouse internt:

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

`FabricClient`är hello-objekt som har använt toocommunicate med hello Service Fabric-klustret för olika hanteringsåtgärder på hello klustret. Detta är användbart när du vill ha mer kontroll över hur en partition DNS-matchare för tjänsten samverkar med klustret. `FabricClient`Utför cachelagring internt och är vanligtvis dyra toocreate så att det är viktigt tooreuse `FabricClient` instanser så mycket som möjligt.

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

En Lös metod är och sedan använda tooretrieve hello-adressen för en tjänst eller en tjänst partition för partitionerade tjänster.

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

En tjänstadress kan lösas med enkelt en ServicePartitionResolver, men det krävs mer arbete tooensure hello matcha adressen kan användas på rätt sätt. Klienten måste toodetect om hello anslutningsförsöket misslyckades på grund av ett tillfälligt fel och kan göras (t.ex. tjänsten flyttas eller inte tillgänglig för tillfället) eller ett permanent fel (t.ex. tjänsten har tagits bort eller hello begärda resursen finns inte längre). En tjänstinstans eller repliker kan flytta från noden toonode när som helst av flera skäl. hello-adress matchas via ServicePartitionResolver kan vara inaktuella hello gång klientkoden försöker tooconnect. I så fall igen hello klienten behöver toore Lös hello-adress. Att tillhandahålla hello tidigare `ResolvedServicePartition` anger som hello lösare måste tootry igen i stället för att hämta en cachelagrad adress.

Normalt behöver hello klientkod inte fungerar med hello ServicePartitionResolver direkt. Det skapas och skickas på toocommunication klienten fabriker i hello Reliable Services API. hello fabriker använder hello matcharen internt toogenerate ett objekt som kan använda toocommunicate med tjänster.

### <a name="communication-clients-and-factories"></a>Klienter för kommunikation och fabriker
hello kommunikation factory biblioteket implementerar ett typiskt försök mönster för hantering av fel som underlättar sprider på nytt anslutningar tooresolved slutpunkter. hello factory-bibliotek innehåller hello återförsöksmekanism medan du tillhandahåller hello felhanterare.

`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`Definierar grundläggande hello gränssnitt som implementerats av en kommunikation klientfabrik som ger klienter som kan kommunicera tooa Service Fabric-tjänsten. Hej implementering av hello CommunicationClientFactory beror på hello kommunikation stack används hello Service Fabric-tjänsten där hello klient vill toocommunicate. hello Reliable Services API ger en `CommunicationClientFactoryBase<TCommunicationClient>`. Detta ger en grundläggande implementering av hello CommunicationClientFactory gränssnitt och utför uppgifter som är vanliga tooall hello kommunikation stackar. (Dessa uppgifter är med hjälp av en tjänstslutpunkt ServicePartitionResolver toodetermine hello). Klienter implementera vanligtvis hello abstrakt CommunicationClientFactoryBase klassen toohandle logiken som är specifika toohello kommunikation stacken.

hello kommunikation klienten bara tar emot en adress och använder den tooconnect tooa service. hello-klienten kan använda det protokoll som den vill.

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

Hej klientfabrik är främst ansvarar för att skapa klienter för kommunikation. För klienter som inte har en beständig anslutning, till exempel en HTTP-klienten behöver bara hello factory toocreate och returnera hello-klienten. Andra protokoll som underhåller en beständig anslutning, till exempel vissa binära protokoll ska också verifieras av hello factory toodetermine om hello anslutningen behöver toobe skapas på nytt.  

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

Slutligen ansvarar en undantagshanterare för att fastställa vilken åtgärd tootake när ett undantag inträffar. Undantag är indelade i **återförsökbart** och **icke återförsökbart**.

* **Ej återförsökbart** undantag bara hämta igen tillbaka toohello anroparen.
* **återförsökbart** undantag kategoriseras i **tillfälligt** och **inte är tillfällig**.
  * **Tillfälligt** undantag är de som bara kan göras utan att återlösa hello slutpunktsadress för tjänsten. Dessa omfattar tillfälliga nätverksproblem eller tjänsten felsvar förutom de som visar hello slutpunktsadress för tjänsten finns inte.
  * **Icke-tillfälligt** undantag är de som kräver hello endpoint adress toobe löst igen. Dessa inkluderar undantag som visar hello tjänstslutpunkten inte kunde nås, som visar hello-tjänsten har flyttat tooa annan nod.

Hej `TryHandleException` gör ett beslut om ett angivet undantag. Om den **inte vet** vilka beslut toomake om ett undantag som den ska returnera **FALSKT**. Om den **känner** vilka beslut toomake som den ska ange hello resultatet i enlighet med detta och returnera **SANT**.

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
### <a name="putting-it-all-together"></a>Alltihop
Med en `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, och `IExceptionHandler(C#) / ExceptionHandler(Java)` uppbyggd kring ett kommunikationsprotokoll en `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` ska radbrytas samtidigt och ger hello fault-hantering och tjänsten partition adress upplösning loop runt dessa komponenter.

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

## <a name="next-steps"></a>Nästa steg
* Se ett exempel på HTTP-kommunikation mellan tjänster i en [C#-exempelprojektet på GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) eller [Java exempelprojektet på GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).
* [RPC-anrop med Reliable Services fjärrkommunikation](service-fabric-reliable-services-communication-remoting.md)
* [Webb-API som använder OWIN i Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-kommunikation med hjälp av Reliable Services](service-fabric-reliable-services-communication-wcf.md)
