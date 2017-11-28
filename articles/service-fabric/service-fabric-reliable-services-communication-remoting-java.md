---
title: "Tjänsten fjärrkommunikation i Azure Service Fabric | Microsoft Docs"
description: "Service Fabric-fjärrkommunikation kan klienter och tjänster att kommunicera med tjänster med hjälp av ett fjärranrop."
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
ms.openlocfilehash: dc4a362b5737bb424ca2c196c85f4c51b6ee5e30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="service-remoting-with-reliable-services"></a><span data-ttu-id="36810-103">Tjänsten fjärrkommunikation med Reliable Services</span><span class="sxs-lookup"><span data-stu-id="36810-103">Service remoting with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="36810-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="36810-104">C# on Windows</span></span>](service-fabric-reliable-services-communication-remoting.md)
> * [<span data-ttu-id="36810-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="36810-105">Java on Linux</span></span>](service-fabric-reliable-services-communication-remoting-java.md)
>
>

<span data-ttu-id="36810-106">Ramverket för Reliable Services innehåller en mekanism för fjärrkommunikation att snabbt och enkelt konfigurera fjärrproceduranrop för tjänster.</span><span class="sxs-lookup"><span data-stu-id="36810-106">The Reliable Services framework provides a remoting mechanism to quickly and easily set up remote procedure call for services.</span></span>

## <a name="set-up-remoting-on-a-service"></a><span data-ttu-id="36810-107">Ställa in fjärrstyrning för en tjänst</span><span class="sxs-lookup"><span data-stu-id="36810-107">Set up remoting on a service</span></span>
<span data-ttu-id="36810-108">Ställa in fjärrstyrning för en tjänst görs i två enkla steg:</span><span class="sxs-lookup"><span data-stu-id="36810-108">Setting up remoting for a service is done in two simple steps:</span></span>

1. <span data-ttu-id="36810-109">Skapa ett gränssnitt för tjänsten att implementera.</span><span class="sxs-lookup"><span data-stu-id="36810-109">Create an interface for your service to implement.</span></span> <span data-ttu-id="36810-110">Det här gränssnittet definierar metoder som är tillgängliga för remote procedure call på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="36810-110">This interface defines the methods that are available for a remote procedure call on your service.</span></span> <span data-ttu-id="36810-111">Metoderna måste vara åtgärdsreturnerande asynkrona metoder.</span><span class="sxs-lookup"><span data-stu-id="36810-111">The methods must be task-returning asynchronous methods.</span></span> <span data-ttu-id="36810-112">Gränssnittet måste implementera `microsoft.serviceFabric.services.remoting.Service` att signalera att tjänsten har ett gränssnitt för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="36810-112">The interface must implement `microsoft.serviceFabric.services.remoting.Service` to signal that the service has a remoting interface.</span></span>
2. <span data-ttu-id="36810-113">Använd en lyssnare för fjärrkommunikation i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="36810-113">Use a remoting listener in your service.</span></span> <span data-ttu-id="36810-114">Det här är en `CommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="36810-114">This is an `CommunicationListener` implementation that provides remoting capabilities.</span></span> <span data-ttu-id="36810-115">`FabricTransportServiceRemotingListener`kan användas för att skapa en lyssnare för fjärrkommunikation med transportprotokollet standard fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="36810-115">`FabricTransportServiceRemotingListener` can be used to create a remoting listener using the default remoting transport protocol.</span></span>

<span data-ttu-id="36810-116">Till exempel exponerar följande tillståndslösa tjänsten en metod för att få ”Hello World” över ett fjärranrop.</span><span class="sxs-lookup"><span data-stu-id="36810-116">For example, the following stateless service exposes a single method to get "Hello World" over a remote procedure call.</span></span>

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
> <span data-ttu-id="36810-117">Argumenten och returtyper i gränssnittet service kan vara enkla, komplexa eller anpassade typer, men de måste kunna serialiseras.</span><span class="sxs-lookup"><span data-stu-id="36810-117">The arguments and the return types in the service interface can be any simple, complex, or custom types, but they must be serializable.</span></span>
>
>

## <a name="call-remote-service-methods"></a><span data-ttu-id="36810-118">Anropa fjärrtjänsten metoder</span><span class="sxs-lookup"><span data-stu-id="36810-118">Call remote service methods</span></span>
<span data-ttu-id="36810-119">Anropar metoder för en tjänst med hjälp av fjärrkommunikation stacken görs med hjälp av en lokal proxyserver till tjänsten via den `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klass.</span><span class="sxs-lookup"><span data-stu-id="36810-119">Calling methods on a service by using the remoting stack is done by using a local proxy to the service through the `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` class.</span></span> <span data-ttu-id="36810-120">Den `ServiceProxyBase` metoden skapar en lokal proxyserver genom att använda samma gränssnitt som tjänsten implementerar.</span><span class="sxs-lookup"><span data-stu-id="36810-120">The `ServiceProxyBase` method creates a local proxy by using the same interface that the service implements.</span></span> <span data-ttu-id="36810-121">Med denna proxy, kan du bara fjärranropa metoder i gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="36810-121">With that proxy, you can simply call methods on the interface remotely.</span></span>

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

<span data-ttu-id="36810-122">Fjärr-framework sprider undantag på tjänsten till klienten.</span><span class="sxs-lookup"><span data-stu-id="36810-122">The remoting framework propagates exceptions thrown at the service to the client.</span></span> <span data-ttu-id="36810-123">Så undantagshantering logik på klienten med hjälp av `ServiceProxyBase` direkt kan hantera undantag som genereras av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="36810-123">So exception-handling logic at the client by using `ServiceProxyBase` can directly handle exceptions that the service throws.</span></span>

## <a name="service-proxy-lifetime"></a><span data-ttu-id="36810-124">Tjänsten Proxylivslängden</span><span class="sxs-lookup"><span data-stu-id="36810-124">Service Proxy Lifetime</span></span>
<span data-ttu-id="36810-125">ServiceProxy skapas en enkel åtgärd, så att användare kan skapa så många som de behöver den.</span><span class="sxs-lookup"><span data-stu-id="36810-125">ServiceProxy creation is a lightweight operation, so user can create as many as they need it.</span></span> <span data-ttu-id="36810-126">Tjänstproxy kan återanvändas så länge användaren behöver den.</span><span class="sxs-lookup"><span data-stu-id="36810-126">Service Proxy can be re-used as long as user need it.</span></span> <span data-ttu-id="36810-127">Användaren kan återanvända samma proxy vid undantag.</span><span class="sxs-lookup"><span data-stu-id="36810-127">User can re-use the same proxy in case of Exception.</span></span> <span data-ttu-id="36810-128">Varje ServiceProxy innehåller kommunikation klient används för att skicka meddelanden via kabel.</span><span class="sxs-lookup"><span data-stu-id="36810-128">Each ServiceProxy contains communication client used to send messages over the wire.</span></span> <span data-ttu-id="36810-129">När du anropar API: T har vi interna kontrollerar om kommunikation klienten använde är giltig.</span><span class="sxs-lookup"><span data-stu-id="36810-129">While invoking API, we have internal check to see if communication client used is valid.</span></span> <span data-ttu-id="36810-130">Baserat på resultatet återskapar vi klienten för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="36810-130">Based on that result, we re-create the communication client.</span></span> <span data-ttu-id="36810-131">Användaren behöver därför inte att återskapa serviceproxy vid undantag.</span><span class="sxs-lookup"><span data-stu-id="36810-131">Hence user do not need to recreate serviceproxy in case of Exception.</span></span>

### <a name="serviceproxyfactory-lifetime"></a><span data-ttu-id="36810-132">ServiceProxyFactory livslängd</span><span class="sxs-lookup"><span data-stu-id="36810-132">ServiceProxyFactory Lifetime</span></span>
<span data-ttu-id="36810-133">[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) är en fabrik som skapar proxy för olika fjärrkommunikation gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="36810-133">[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) is a factory that creates proxy for different remoting interfaces.</span></span> <span data-ttu-id="36810-134">Om du använder API `ServiceProxyBase.create` för att skapa proxy, framework skapar sedan en `FabricServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="36810-134">If you use API `ServiceProxyBase.create` for creating proxy, then framework creates a `FabricServiceProxyFactory`.</span></span>
<span data-ttu-id="36810-135">Det är praktiskt att skapa ett manuellt när du måste åsidosätta [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) egenskaper.</span><span class="sxs-lookup"><span data-stu-id="36810-135">It is useful to create one manually when you need to override [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) properties.</span></span>
<span data-ttu-id="36810-136">Fabriken är en kostsam åtgärd.</span><span class="sxs-lookup"><span data-stu-id="36810-136">Factory is an expensive operation.</span></span> <span data-ttu-id="36810-137">`FabricServiceProxyFactory`underhåller cache för klienter för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="36810-137">`FabricServiceProxyFactory` maintains cache of communication clients.</span></span>
<span data-ttu-id="36810-138">Bästa praxis är att cacheminnet `FabricServiceProxyFactory` så länge som möjligt.</span><span class="sxs-lookup"><span data-stu-id="36810-138">Best practice is to cache `FabricServiceProxyFactory` for as long as possible.</span></span>

## <a name="remoting-exception-handling"></a><span data-ttu-id="36810-139">Fjärrkommunikation undantagshantering</span><span class="sxs-lookup"><span data-stu-id="36810-139">Remoting Exception Handling</span></span>
<span data-ttu-id="36810-140">Alla fjärranslutna undantag som uppstått i tjänsten API, skickas tillbaka till klienten som RuntimeException eller FabricException.</span><span class="sxs-lookup"><span data-stu-id="36810-140">All the remote exception thrown by service API, are sent back to the client either as RuntimeException or FabricException.</span></span>

<span data-ttu-id="36810-141">ServiceProxy hanterar alla Failover-undantag för service-partition som skapats för.</span><span class="sxs-lookup"><span data-stu-id="36810-141">ServiceProxy does handle all Failover Exception for the service partition it  is created for.</span></span> <span data-ttu-id="36810-142">Slutpunkterna löser igen om det finns Failover Exceptions(Non-Transient Exceptions) och försöker anropa med korrekt slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="36810-142">It re-resolves the endpoints if there is Failover Exceptions(Non-Transient Exceptions) and retries the call with the correct endpoint.</span></span> <span data-ttu-id="36810-143">Antal återförsök för redundans undantag är obegränsad.</span><span class="sxs-lookup"><span data-stu-id="36810-143">Number of retries for failover Exception is indefinite.</span></span>
<span data-ttu-id="36810-144">Vid TransientExceptions, endast ett nytt försök anropet.</span><span class="sxs-lookup"><span data-stu-id="36810-144">In case of TransientExceptions, it only retries the call.</span></span>

<span data-ttu-id="36810-145">Försök standardparametrarna är etablerar av [OperationRetrySettings].</span><span class="sxs-lookup"><span data-stu-id="36810-145">Default retry parameters are provied by [OperationRetrySettings].</span></span> <span data-ttu-id="36810-146">(https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Användare kan konfigurera dessa värden genom att skicka OperationRetrySettings objekt till ServiceProxyFactory konstruktor.</span><span class="sxs-lookup"><span data-stu-id="36810-146">(https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) User can configure these values by passing OperationRetrySettings object to ServiceProxyFactory constructor.</span></span>

## <a name="next-steps"></a><span data-ttu-id="36810-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="36810-147">Next steps</span></span>
* [<span data-ttu-id="36810-148">Att säkra kommunikationen för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="36810-148">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)
