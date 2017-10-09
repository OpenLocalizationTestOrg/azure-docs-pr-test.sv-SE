---
title: "aaaService fjärrkommunikation i Service Fabric | Microsoft Docs"
description: "Service Fabric-fjärrkommunikation tillåter att klienter och tjänster toocommunicate med tjänster med hjälp av ett fjärranrop."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: vturecek
ms.openlocfilehash: 14682cf8671a85e04144eccf97803ab67c258875
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-remoting-with-reliable-services"></a><span data-ttu-id="4a23a-103">Tjänsten fjärrkommunikation med Reliable Services</span><span class="sxs-lookup"><span data-stu-id="4a23a-103">Service remoting with Reliable Services</span></span>
<span data-ttu-id="4a23a-104">För tjänster som inte är knutna tooa viss kommunikationsprotokoll eller stacken, till exempel WebAPI, Windows Communication Foundation (WCF) eller andra hello Reliable Services framework tillhandahåller en mekanism tooquickly för fjärrkommunikation och konfigurera enkelt fjärrproceduranrop för tjänster.</span><span class="sxs-lookup"><span data-stu-id="4a23a-104">For services that are not tied tooa particular communication protocol or stack, such as WebAPI, Windows Communication Foundation (WCF), or others, hello Reliable Services framework provides a remoting mechanism tooquickly and easily set up remote procedure call for services.</span></span>

## <a name="set-up-remoting-on-a-service"></a><span data-ttu-id="4a23a-105">Ställa in fjärrstyrning för en tjänst</span><span class="sxs-lookup"><span data-stu-id="4a23a-105">Set up remoting on a service</span></span>
<span data-ttu-id="4a23a-106">Ställa in fjärrstyrning för en tjänst görs i två enkla steg:</span><span class="sxs-lookup"><span data-stu-id="4a23a-106">Setting up remoting for a service is done in two simple steps:</span></span>

1. <span data-ttu-id="4a23a-107">Skapa ett gränssnitt för service-tooimplement.</span><span class="sxs-lookup"><span data-stu-id="4a23a-107">Create an interface for your service tooimplement.</span></span> <span data-ttu-id="4a23a-108">Det här gränssnittet definierar hello-metoder som är tillgängliga för remote procedure call på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="4a23a-108">This interface defines hello methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="4a23a-109">hello-metoder måste vara åtgärdsreturnerande asynkrona metoder.</span><span class="sxs-lookup"><span data-stu-id="4a23a-109">hello methods must be task-returning asynchronous methods.</span></span> <span data-ttu-id="4a23a-110">hello gränssnittet måste implementera `Microsoft.ServiceFabric.Services.Remoting.IService` toosignal som hello tjänst har ett gränssnitt för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="4a23a-110">hello interface must implement `Microsoft.ServiceFabric.Services.Remoting.IService` toosignal that hello service has a remoting interface.</span></span>
2. <span data-ttu-id="4a23a-111">Använd en lyssnare för fjärrkommunikation i din tjänst.</span><span class="sxs-lookup"><span data-stu-id="4a23a-111">Use a remoting listener in your service.</span></span> <span data-ttu-id="4a23a-112">Det här är en `ICommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="4a23a-112">This is an `ICommunicationListener` implementation that provides remoting capabilities.</span></span> <span data-ttu-id="4a23a-113">Hej `Microsoft.ServiceFabric.Services.Remoting.Runtime` namnområde innehåller en metod,`CreateServiceRemotingListener` för både tillståndslösa och tillståndskänsliga tjänster som kan vara används toocreate en lyssnare för fjärrkommunikation med hello standard fjärrkommunikation transportprotokollet.</span><span class="sxs-lookup"><span data-stu-id="4a23a-113">hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` namespace contains an extension method,`CreateServiceRemotingListener` for both stateless and stateful services that can be used toocreate a remoting listener using hello default remoting transport protocol.</span></span>

<span data-ttu-id="4a23a-114">Obs: hello `Remoting` namnområdet är tillgänglig som ett separat nuget-paket som kallas`Microsoft.ServiceFabric.Services.Remoting`</span><span class="sxs-lookup"><span data-stu-id="4a23a-114">Note: hello `Remoting` namespace is available as a seperate nuget package called `Microsoft.ServiceFabric.Services.Remoting`</span></span> 

<span data-ttu-id="4a23a-115">Till exempel exponerar hello följande tillståndslösa tjänsten en enda metod tooget ”Hello World” över ett fjärranrop.</span><span class="sxs-lookup"><span data-stu-id="4a23a-115">For example, hello following stateless service exposes a single method tooget "Hello World" over a remote procedure call.</span></span>

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context =>            this.CreateServiceRemotingListener(context)) };
    }
}
```
> [!NOTE]
> <span data-ttu-id="4a23a-116">hello argument och hello returnera typer i hello service-gränssnittet kan vara enkla, komplexa eller anpassade typer, men de måste kunna serialiseras med hello .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).</span><span class="sxs-lookup"><span data-stu-id="4a23a-116">hello arguments and hello return types in hello service interface can be any simple, complex, or custom types, but they must be serializable by hello .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).</span></span>
>
>

## <a name="call-remote-service-methods"></a><span data-ttu-id="4a23a-117">Anropa fjärrtjänsten metoder</span><span class="sxs-lookup"><span data-stu-id="4a23a-117">Call remote service methods</span></span>
<span data-ttu-id="4a23a-118">Anropar metoder för en tjänst med hjälp av hello fjärrkommunikation stack görs med hjälp av en lokal proxyserver toohello tjänst via hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klass.</span><span class="sxs-lookup"><span data-stu-id="4a23a-118">Calling methods on a service by using hello remoting stack is done by using a local proxy toohello service through hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` class.</span></span> <span data-ttu-id="4a23a-119">Hej `ServiceProxy` metoden skapar en lokal proxyserver genom att använda samma gränssnitt som hello tjänsten implementerar hello.</span><span class="sxs-lookup"><span data-stu-id="4a23a-119">hello `ServiceProxy` method creates a local proxy by using hello same interface that hello service implements.</span></span> <span data-ttu-id="4a23a-120">Med denna proxy, kan du bara fjärranropa metoder i hello gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="4a23a-120">With that proxy, you can simply call methods on hello interface remotely.</span></span>

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

<span data-ttu-id="4a23a-121">hello fjärrkommunikation framework sprider undantag på klienten toohello hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4a23a-121">hello remoting framework propagates exceptions thrown at hello service toohello client.</span></span> <span data-ttu-id="4a23a-122">Så undantagshantering logik på hello-klient med hjälp av `ServiceProxy` direkt kan hantera undantag som hello tjänsten returnerar.</span><span class="sxs-lookup"><span data-stu-id="4a23a-122">So exception-handling logic at hello client by using `ServiceProxy` can directly handle exceptions that hello service throws.</span></span>

## <a name="service-proxy-lifetime"></a><span data-ttu-id="4a23a-123">Tjänsten Proxylivslängden</span><span class="sxs-lookup"><span data-stu-id="4a23a-123">Service Proxy Lifetime</span></span>
<span data-ttu-id="4a23a-124">ServiceProxy skapas en enkel åtgärd, så att användare kan skapa så många som de behöver den.</span><span class="sxs-lookup"><span data-stu-id="4a23a-124">ServiceProxy creation is a lightweight operation, so user can create as many as they need it.</span></span> <span data-ttu-id="4a23a-125">Tjänstproxy kan återanvändas så länge användaren behöver den.</span><span class="sxs-lookup"><span data-stu-id="4a23a-125">Service Proxy can be re-used as long as user need it.</span></span> <span data-ttu-id="4a23a-126">Användaren kan återanvända hello samma proxy vid undantag.</span><span class="sxs-lookup"><span data-stu-id="4a23a-126">User can re-use hello same proxy in case of Exception.</span></span> <span data-ttu-id="4a23a-127">Varje ServiceProxy innehåller kommunikation platser klienten använde toosend meddelanden över hello-överföring.</span><span class="sxs-lookup"><span data-stu-id="4a23a-127">Each ServiceProxy contains communciation client used toosend messages over hello wire.</span></span> <span data-ttu-id="4a23a-128">När du anropar API: T har vi interna kontrollen toosee om kommunikation klienten använde är giltig.</span><span class="sxs-lookup"><span data-stu-id="4a23a-128">While invoking API, we have internal check toosee if communication client used is valid.</span></span> <span data-ttu-id="4a23a-129">Baserat på resultatet återskapar vi hello kommunikation klienten.</span><span class="sxs-lookup"><span data-stu-id="4a23a-129">Based on that result, we re-create hello communication client.</span></span> <span data-ttu-id="4a23a-130">Därför behöver inte användaren toorecreate serviceproxy vid undantag.</span><span class="sxs-lookup"><span data-stu-id="4a23a-130">Hence user do not need toorecreate serviceproxy in case of Exception.</span></span>

### <a name="serviceproxyfactory-lifetime"></a><span data-ttu-id="4a23a-131">ServiceProxyFactory livslängd</span><span class="sxs-lookup"><span data-stu-id="4a23a-131">ServiceProxyFactory Lifetime</span></span>
<span data-ttu-id="4a23a-132">[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) är en fabrik som skapar proxy för olika fjärrkommunikation gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="4a23a-132">[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) is a factory that creates proxy for different remoting interfaces.</span></span> <span data-ttu-id="4a23a-133">Om du använder API ServiceProxy.Create för att skapa proxy skapar framework hello singelton ServiceProxyFactory.</span><span class="sxs-lookup"><span data-stu-id="4a23a-133">If you use API ServiceProxy.Create for creating proxy, then framework creates hello singelton ServiceProxyFactory.</span></span>
<span data-ttu-id="4a23a-134">Det är användbart toocreate en manuellt när du behöver toooverride [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) egenskaper.</span><span class="sxs-lookup"><span data-stu-id="4a23a-134">It is useful toocreate one manually when you need toooverride [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) properties.</span></span>
<span data-ttu-id="4a23a-135">Fabriken är en kostsam åtgärd.</span><span class="sxs-lookup"><span data-stu-id="4a23a-135">Factory is an expensive operation.</span></span> <span data-ttu-id="4a23a-136">ServiceProxyFactory underhåller cacheminnet på klienten för kommunikation.</span><span class="sxs-lookup"><span data-stu-id="4a23a-136">ServiceProxyFactory maintains cache of communication client.</span></span>
<span data-ttu-id="4a23a-137">Bästa praxis är toocache ServiceProxyFactory så länge som möjligt.</span><span class="sxs-lookup"><span data-stu-id="4a23a-137">Best practice is toocache ServiceProxyFactory for as long as possible.</span></span>

## <a name="remoting-exception-handling"></a><span data-ttu-id="4a23a-138">Fjärrkommunikation undantagshantering</span><span class="sxs-lookup"><span data-stu-id="4a23a-138">Remoting Exception Handling</span></span>
<span data-ttu-id="4a23a-139">Alla hello remote undantagsfel från service API, skickas tillbaka toohello klienten som AggregateException.</span><span class="sxs-lookup"><span data-stu-id="4a23a-139">All hello remote exception thrown by service API, are sent back toohello client as AggregateException.</span></span> <span data-ttu-id="4a23a-140">RemoteExceptions ska kunna serialiseras DataContract annars [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) genereras toohello proxy API med hello serialiseringsfel i den.</span><span class="sxs-lookup"><span data-stu-id="4a23a-140">RemoteExceptions should be DataContract Serializable otherwise [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) is thrown toohello proxy API with hello serialization error in it.</span></span>

<span data-ttu-id="4a23a-141">ServiceProxy hanterar alla redundans undantag för hello service partition som skapats för.</span><span class="sxs-lookup"><span data-stu-id="4a23a-141">ServiceProxy does handle all Failover Exception for hello service partition it  is created for.</span></span> <span data-ttu-id="4a23a-142">Hello slutpunkter löser igen om det finns Failover Exceptions(Non-Transient Exceptions) och försöker hello-anrop med hello korrekt slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="4a23a-142">It re-resolves hello endpoints if there is Failover Exceptions(Non-Transient Exceptions) and retries hello call with hello correct endpoint.</span></span> <span data-ttu-id="4a23a-143">Antal återförsök för redundans undantag är obegränsad.</span><span class="sxs-lookup"><span data-stu-id="4a23a-143">Number of retries for failover Exception is indefinite.</span></span>
<span data-ttu-id="4a23a-144">Vid TransientExceptions, endast ett nytt försök hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="4a23a-144">In case of TransientExceptions, it only retries hello call.</span></span>

<span data-ttu-id="4a23a-145">Försök standardparametrarna är etablerar av [OperationRetrySettings].</span><span class="sxs-lookup"><span data-stu-id="4a23a-145">Default retry parameters are provied by [OperationRetrySettings].</span></span> <span data-ttu-id="4a23a-146">(https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Användare kan konfigurera dessa värden genom att skicka OperationRetrySettings objektet tooServiceProxyFactory konstruktor.</span><span class="sxs-lookup"><span data-stu-id="4a23a-146">(https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) User can configure these values by passing OperationRetrySettings object tooServiceProxyFactory constructor.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4a23a-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a23a-147">Next steps</span></span>
* [<span data-ttu-id="4a23a-148">Webb-API med OWIN i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="4a23a-148">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="4a23a-149">WCF-kommunikation med Reliable Services</span><span class="sxs-lookup"><span data-stu-id="4a23a-149">WCF communication with Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
* [<span data-ttu-id="4a23a-150">Att säkra kommunikationen för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="4a23a-150">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)
