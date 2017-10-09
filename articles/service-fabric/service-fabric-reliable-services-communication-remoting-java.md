---
title: "aaaService fjärrkommunikation i Azure Service Fabric | Microsoft Docs"
description: "Service Fabric-fjärrkommunikation tillåter att klienter och tjänster toocommunicate med tjänster med hjälp av ett fjärranrop."
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 1177a5ede91352dc61422f2df7424b0d5645147d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-remoting-with-reliable-services"></a><span data-ttu-id="81bfd-103">Tjänsten fjärrkommunikation med Reliable Services</span><span class="sxs-lookup"><span data-stu-id="81bfd-103">Service remoting with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="81bfd-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="81bfd-104">C# on Windows</span></span>](service-fabric-reliable-services-communication-remoting.md)
> * [<span data-ttu-id="81bfd-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="81bfd-105">Java on Linux</span></span>](service-fabric-reliable-services-communication-remoting-java.md)
>
>

<span data-ttu-id="81bfd-106">hello Reliable Services framework tillhandahåller en mekanism tooquickly för fjärrkommunikation och konfigurera enkelt fjärrproceduranrop för tjänster.</span><span class="sxs-lookup"><span data-stu-id="81bfd-106">hello Reliable Services framework provides a remoting mechanism tooquickly and easily set up remote procedure call for services.</span></span>

## <a name="set-up-remoting-on-a-service"></a><span data-ttu-id="81bfd-107">Ställa in fjärrstyrning för en tjänst</span><span class="sxs-lookup"><span data-stu-id="81bfd-107">Set up remoting on a service</span></span>
<span data-ttu-id="81bfd-108">Ställa in fjärrstyrning för en tjänst görs i två enkla steg:</span><span class="sxs-lookup"><span data-stu-id="81bfd-108">Setting up remoting for a service is done in two simple steps:</span></span>

1. <span data-ttu-id="81bfd-109">Skapa ett gränssnitt för service-tooimplement.</span><span class="sxs-lookup"><span data-stu-id="81bfd-109">Create an interface for your service tooimplement.</span></span> <span data-ttu-id="81bfd-110">Det här gränssnittet definierar hello-metoder som är tillgängliga för remote procedure call på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="81bfd-110">This interface defines hello methods that are available for a remote procedure call on your service.</span></span> <span data-ttu-id="81bfd-111">hello-metoder måste vara åtgärdsreturnerande asynkrona metoder.</span><span class="sxs-lookup"><span data-stu-id="81bfd-111">hello methods must be task-returning asynchronous methods.</span></span> <span data-ttu-id="81bfd-112">hello gränssnittet måste implementera `microsoft.serviceFabric.services.remoting.Service` toosignal som hello tjänst har ett gränssnitt för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="81bfd-112">hello interface must implement `microsoft.serviceFabric.services.remoting.Service` toosignal that hello service has a remoting interface.</span></span>
2. <span data-ttu-id="81bfd-113">Använd en lyssnare för fjärrkommunikation i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="81bfd-113">Use a remoting listener in your service.</span></span> <span data-ttu-id="81bfd-114">Det här är en `CommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="81bfd-114">This is an `CommunicationListener` implementation that provides remoting capabilities.</span></span> <span data-ttu-id="81bfd-115">`FabricTransportServiceRemotingListener`kan vara används toocreate en lyssnare för fjärrkommunikation med transportprotokollet för hello standard fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="81bfd-115">`FabricTransportServiceRemotingListener` can be used toocreate a remoting listener using hello default remoting transport protocol.</span></span>

<span data-ttu-id="81bfd-116">Till exempel exponerar hello följande tillståndslösa tjänsten en enda metod tooget ”Hello World” över ett fjärranrop.</span><span class="sxs-lookup"><span data-stu-id="81bfd-116">For example, hello following stateless service exposes a single method tooget "Hello World" over a remote procedure call.</span></span>

```java
import java.util.ArrayList;
import java.util.concurrent.CompletableFuture;
import java.util.List;
import microsoft.servicefabric.services.communication.runtime.ServiceInstanceListener;
import microsoft.servicefabric.services.remoting.Service;
import microsoft.servicefabric.services.runtime.StatelessService;

public interface MyService extends Service {
    CompletableFuture<String> helloWorldAsync();
}

class MyServiceImpl extends StatelessService implements MyService {
    public MyServiceImpl(StatelessServiceContext context) {
       super(context);
    }

    public CompletableFuture<String> helloWorldAsync() {
        return CompletableFuture.completedFuture("Hello!");
    }

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
        listeners.add(new ServiceInstanceListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));
        return listeners;
    }
}
```

> [!NOTE]
> <span data-ttu-id="81bfd-117">hello argument och hello returnera typer i hello service-gränssnittet kan vara enkla, komplexa eller anpassade typer, men de måste kunna serialiseras.</span><span class="sxs-lookup"><span data-stu-id="81bfd-117">hello arguments and hello return types in hello service interface can be any simple, complex, or custom types, but they must be serializable.</span></span>
>
>

## <a name="call-remote-service-methods"></a><span data-ttu-id="81bfd-118">Anropa fjärrtjänsten metoder</span><span class="sxs-lookup"><span data-stu-id="81bfd-118">Call remote service methods</span></span>
<span data-ttu-id="81bfd-119">Anropar metoder för en tjänst med hjälp av hello fjärrkommunikation stack görs med hjälp av en lokal proxyserver toohello tjänst via hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klass.</span><span class="sxs-lookup"><span data-stu-id="81bfd-119">Calling methods on a service by using hello remoting stack is done by using a local proxy toohello service through hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` class.</span></span> <span data-ttu-id="81bfd-120">Hej `ServiceProxyBase` metoden skapar en lokal proxyserver genom att använda samma gränssnitt som hello tjänsten implementerar hello.</span><span class="sxs-lookup"><span data-stu-id="81bfd-120">hello `ServiceProxyBase` method creates a local proxy by using hello same interface that hello service implements.</span></span> <span data-ttu-id="81bfd-121">Med denna proxy, kan du bara fjärranropa metoder i hello gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="81bfd-121">With that proxy, you can simply call methods on hello interface remotely.</span></span>

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

<span data-ttu-id="81bfd-122">hello fjärrkommunikation framework sprider undantag på klienten toohello hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="81bfd-122">hello remoting framework propagates exceptions thrown at hello service toohello client.</span></span> <span data-ttu-id="81bfd-123">Så undantagshantering logik på hello-klient med hjälp av `ServiceProxyBase` direkt kan hantera undantag som hello tjänsten returnerar.</span><span class="sxs-lookup"><span data-stu-id="81bfd-123">So exception-handling logic at hello client by using `ServiceProxyBase` can directly handle exceptions that hello service throws.</span></span>

## <a name="service-proxy-lifetime"></a><span data-ttu-id="81bfd-124">Tjänsten Proxylivslängden</span><span class="sxs-lookup"><span data-stu-id="81bfd-124">Service Proxy Lifetime</span></span>
<span data-ttu-id="81bfd-125">ServiceProxy skapas en enkel åtgärd, så att användare kan skapa så många som de behöver den.</span><span class="sxs-lookup"><span data-stu-id="81bfd-125">ServiceProxy creation is a lightweight operation, so user can create as many as they need it.</span></span> <span data-ttu-id="81bfd-126">Tjänstproxy kan återanvändas så länge användaren behöver den.</span><span class="sxs-lookup"><span data-stu-id="81bfd-126">Service Proxy can be re-used as long as user need it.</span></span> <span data-ttu-id="81bfd-127">Användaren kan återanvända hello samma proxy vid undantag.</span><span class="sxs-lookup"><span data-stu-id="81bfd-127">User can re-use hello same proxy in case of Exception.</span></span> <span data-ttu-id="81bfd-128">Varje ServiceProxy innehåller kommunikation klienten använde toosend meddelanden över hello-överföring.</span><span class="sxs-lookup"><span data-stu-id="81bfd-128">Each ServiceProxy contains communication client used toosend messages over hello wire.</span></span> <span data-ttu-id="81bfd-129">När du anropar API: T har vi interna kontrollen toosee om kommunikation klienten använde är giltig.</span><span class="sxs-lookup"><span data-stu-id="81bfd-129">While invoking API, we have internal check toosee if communication client used is valid.</span></span> <span data-ttu-id="81bfd-130">Baserat på resultatet återskapar vi hello kommunikation klienten.</span><span class="sxs-lookup"><span data-stu-id="81bfd-130">Based on that result, we re-create hello communication client.</span></span> <span data-ttu-id="81bfd-131">Därför behöver inte användaren toorecreate serviceproxy vid undantag.</span><span class="sxs-lookup"><span data-stu-id="81bfd-131">Hence user do not need toorecreate serviceproxy in case of Exception.</span></span>

### <a name="serviceproxyfactory-lifetime"></a><span data-ttu-id="81bfd-132">ServiceProxyFactory livslängd</span><span class="sxs-lookup"><span data-stu-id="81bfd-132">ServiceProxyFactory Lifetime</span></span>
<span data-ttu-id="81bfd-133">[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) är en fabrik som skapar proxy för olika fjärrkommunikation gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="81bfd-133">[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) is a factory that creates proxy for different remoting interfaces.</span></span> <span data-ttu-id="81bfd-134">Om du använder API `ServiceProxyBase.create` för att skapa proxy, framework skapar sedan en `FabricServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="81bfd-134">If you use API `ServiceProxyBase.create` for creating proxy, then framework creates a `FabricServiceProxyFactory`.</span></span>
<span data-ttu-id="81bfd-135">Det är användbart toocreate en manuellt när du behöver toooverride [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) egenskaper.</span><span class="sxs-lookup"><span data-stu-id="81bfd-135">It is useful toocreate one manually when you need toooverride [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) properties.</span></span>
<span data-ttu-id="81bfd-136">Fabriken är en kostsam åtgärd.</span><span class="sxs-lookup"><span data-stu-id="81bfd-136">Factory is an expensive operation.</span></span> <span data-ttu-id="81bfd-137">`FabricServiceProxyFactory`underhåller cache för klienter för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="81bfd-137">`FabricServiceProxyFactory` maintains cache of communication clients.</span></span>
<span data-ttu-id="81bfd-138">Bästa praxis är toocache `FabricServiceProxyFactory` så länge som möjligt.</span><span class="sxs-lookup"><span data-stu-id="81bfd-138">Best practice is toocache `FabricServiceProxyFactory` for as long as possible.</span></span>

## <a name="remoting-exception-handling"></a><span data-ttu-id="81bfd-139">Fjärrkommunikation undantagshantering</span><span class="sxs-lookup"><span data-stu-id="81bfd-139">Remoting Exception Handling</span></span>
<span data-ttu-id="81bfd-140">Alla hello remote undantagsfel från service API, skickas tillbaka toohello klienten som RuntimeException eller FabricException.</span><span class="sxs-lookup"><span data-stu-id="81bfd-140">All hello remote exception thrown by service API, are sent back toohello client either as RuntimeException or FabricException.</span></span>

<span data-ttu-id="81bfd-141">ServiceProxy hanterar alla redundans undantag för hello service partition som skapats för.</span><span class="sxs-lookup"><span data-stu-id="81bfd-141">ServiceProxy does handle all Failover Exception for hello service partition it  is created for.</span></span> <span data-ttu-id="81bfd-142">Hello slutpunkter löser igen om det finns Failover Exceptions(Non-Transient Exceptions) och försöker hello-anrop med hello korrekt slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="81bfd-142">It re-resolves hello endpoints if there is Failover Exceptions(Non-Transient Exceptions) and retries hello call with hello correct endpoint.</span></span> <span data-ttu-id="81bfd-143">Antal återförsök för redundans undantag är obegränsad.</span><span class="sxs-lookup"><span data-stu-id="81bfd-143">Number of retries for failover Exception is indefinite.</span></span>
<span data-ttu-id="81bfd-144">Vid TransientExceptions, endast ett nytt försök hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="81bfd-144">In case of TransientExceptions, it only retries hello call.</span></span>

<span data-ttu-id="81bfd-145">Försök standardparametrarna är etablerar av [OperationRetrySettings].</span><span class="sxs-lookup"><span data-stu-id="81bfd-145">Default retry parameters are provied by [OperationRetrySettings].</span></span> <span data-ttu-id="81bfd-146">(https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Användare kan konfigurera dessa värden genom att skicka OperationRetrySettings objektet tooServiceProxyFactory konstruktor.</span><span class="sxs-lookup"><span data-stu-id="81bfd-146">(https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) User can configure these values by passing OperationRetrySettings object tooServiceProxyFactory constructor.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81bfd-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81bfd-147">Next steps</span></span>
* [<span data-ttu-id="81bfd-148">Att säkra kommunikationen för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="81bfd-148">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)
