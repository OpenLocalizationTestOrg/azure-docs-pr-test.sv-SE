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
# <a name="service-remoting-with-reliable-services"></a>Tjänsten fjärrkommunikation med Reliable Services
För tjänster som inte är knutna tooa viss kommunikationsprotokoll eller stacken, till exempel WebAPI, Windows Communication Foundation (WCF) eller andra hello Reliable Services framework tillhandahåller en mekanism tooquickly för fjärrkommunikation och konfigurera enkelt fjärrproceduranrop för tjänster.

## <a name="set-up-remoting-on-a-service"></a>Ställa in fjärrstyrning för en tjänst
Ställa in fjärrstyrning för en tjänst görs i två enkla steg:

1. Skapa ett gränssnitt för service-tooimplement. Det här gränssnittet definierar hello-metoder som är tillgängliga för remote procedure call på din tjänst. hello-metoder måste vara åtgärdsreturnerande asynkrona metoder. hello gränssnittet måste implementera `Microsoft.ServiceFabric.Services.Remoting.IService` toosignal som hello tjänst har ett gränssnitt för fjärrkommunikation.
2. Använd en lyssnare för fjärrkommunikation i din tjänst. Det här är en `ICommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation. Hej `Microsoft.ServiceFabric.Services.Remoting.Runtime` namnområde innehåller en metod,`CreateServiceRemotingListener` för både tillståndslösa och tillståndskänsliga tjänster som kan vara används toocreate en lyssnare för fjärrkommunikation med hello standard fjärrkommunikation transportprotokollet.

Obs: hello `Remoting` namnområdet är tillgänglig som ett separat nuget-paket som kallas`Microsoft.ServiceFabric.Services.Remoting` 

Till exempel exponerar hello följande tillståndslösa tjänsten en enda metod tooget ”Hello World” över ett fjärranrop.

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
> hello argument och hello returnera typer i hello service-gränssnittet kan vara enkla, komplexa eller anpassade typer, men de måste kunna serialiseras med hello .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Anropa fjärrtjänsten metoder
Anropar metoder för en tjänst med hjälp av hello fjärrkommunikation stack görs med hjälp av en lokal proxyserver toohello tjänst via hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klass. Hej `ServiceProxy` metoden skapar en lokal proxyserver genom att använda samma gränssnitt som hello tjänsten implementerar hello. Med denna proxy, kan du bara fjärranropa metoder i hello gränssnitt.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

hello fjärrkommunikation framework sprider undantag på klienten toohello hello-tjänsten. Så undantagshantering logik på hello-klient med hjälp av `ServiceProxy` direkt kan hantera undantag som hello tjänsten returnerar.

## <a name="service-proxy-lifetime"></a>Tjänsten Proxylivslängden
ServiceProxy skapas en enkel åtgärd, så att användare kan skapa så många som de behöver den. Tjänstproxy kan återanvändas så länge användaren behöver den. Användaren kan återanvända hello samma proxy vid undantag. Varje ServiceProxy innehåller kommunikation platser klienten använde toosend meddelanden över hello-överföring. När du anropar API: T har vi interna kontrollen toosee om kommunikation klienten använde är giltig. Baserat på resultatet återskapar vi hello kommunikation klienten. Därför behöver inte användaren toorecreate serviceproxy vid undantag.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory livslängd
[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) är en fabrik som skapar proxy för olika fjärrkommunikation gränssnitt. Om du använder API ServiceProxy.Create för att skapa proxy skapar framework hello singelton ServiceProxyFactory.
Det är användbart toocreate en manuellt när du behöver toooverride [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) egenskaper.
Fabriken är en kostsam åtgärd. ServiceProxyFactory underhåller cacheminnet på klienten för kommunikation.
Bästa praxis är toocache ServiceProxyFactory så länge som möjligt.

## <a name="remoting-exception-handling"></a>Fjärrkommunikation undantagshantering
Alla hello remote undantagsfel från service API, skickas tillbaka toohello klienten som AggregateException. RemoteExceptions ska kunna serialiseras DataContract annars [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) genereras toohello proxy API med hello serialiseringsfel i den.

ServiceProxy hanterar alla redundans undantag för hello service partition som skapats för. Hello slutpunkter löser igen om det finns Failover Exceptions(Non-Transient Exceptions) och försöker hello-anrop med hello korrekt slutpunkt. Antal återförsök för redundans undantag är obegränsad.
Vid TransientExceptions, endast ett nytt försök hello-anrop.

Försök standardparametrarna är etablerar av [OperationRetrySettings]. (https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Användare kan konfigurera dessa värden genom att skicka OperationRetrySettings objektet tooServiceProxyFactory konstruktor.

## <a name="next-steps"></a>Nästa steg
* [Webb-API med OWIN i Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-kommunikation med Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [Att säkra kommunikationen för Reliable Services](service-fabric-reliable-services-secure-communication.md)
