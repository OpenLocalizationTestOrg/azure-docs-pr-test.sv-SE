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
# <a name="service-remoting-with-reliable-services"></a>Tjänsten fjärrkommunikation med Reliable Services
> [!div class="op_single_selector"]
> * [C# i Windows](service-fabric-reliable-services-communication-remoting.md)
> * [Java i Linux](service-fabric-reliable-services-communication-remoting-java.md)
>
>

hello Reliable Services framework tillhandahåller en mekanism tooquickly för fjärrkommunikation och konfigurera enkelt fjärrproceduranrop för tjänster.

## <a name="set-up-remoting-on-a-service"></a>Ställa in fjärrstyrning för en tjänst
Ställa in fjärrstyrning för en tjänst görs i två enkla steg:

1. Skapa ett gränssnitt för service-tooimplement. Det här gränssnittet definierar hello-metoder som är tillgängliga för remote procedure call på din tjänst. hello-metoder måste vara åtgärdsreturnerande asynkrona metoder. hello gränssnittet måste implementera `microsoft.serviceFabric.services.remoting.Service` toosignal som hello tjänst har ett gränssnitt för fjärrkommunikation.
2. Använd en lyssnare för fjärrkommunikation i din tjänst. Det här är en `CommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation. `FabricTransportServiceRemotingListener`kan vara används toocreate en lyssnare för fjärrkommunikation med transportprotokollet för hello standard fjärrkommunikation.

Till exempel exponerar hello följande tillståndslösa tjänsten en enda metod tooget ”Hello World” över ett fjärranrop.

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
> hello argument och hello returnera typer i hello service-gränssnittet kan vara enkla, komplexa eller anpassade typer, men de måste kunna serialiseras.
>
>

## <a name="call-remote-service-methods"></a>Anropa fjärrtjänsten metoder
Anropar metoder för en tjänst med hjälp av hello fjärrkommunikation stack görs med hjälp av en lokal proxyserver toohello tjänst via hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klass. Hej `ServiceProxyBase` metoden skapar en lokal proxyserver genom att använda samma gränssnitt som hello tjänsten implementerar hello. Med denna proxy, kan du bara fjärranropa metoder i hello gränssnitt.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

hello fjärrkommunikation framework sprider undantag på klienten toohello hello-tjänsten. Så undantagshantering logik på hello-klient med hjälp av `ServiceProxyBase` direkt kan hantera undantag som hello tjänsten returnerar.

## <a name="service-proxy-lifetime"></a>Tjänsten Proxylivslängden
ServiceProxy skapas en enkel åtgärd, så att användare kan skapa så många som de behöver den. Tjänstproxy kan återanvändas så länge användaren behöver den. Användaren kan återanvända hello samma proxy vid undantag. Varje ServiceProxy innehåller kommunikation klienten använde toosend meddelanden över hello-överföring. När du anropar API: T har vi interna kontrollen toosee om kommunikation klienten använde är giltig. Baserat på resultatet återskapar vi hello kommunikation klienten. Därför behöver inte användaren toorecreate serviceproxy vid undantag.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory livslängd
[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) är en fabrik som skapar proxy för olika fjärrkommunikation gränssnitt. Om du använder API `ServiceProxyBase.create` för att skapa proxy, framework skapar sedan en `FabricServiceProxyFactory`.
Det är användbart toocreate en manuellt när du behöver toooverride [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) egenskaper.
Fabriken är en kostsam åtgärd. `FabricServiceProxyFactory`underhåller cache för klienter för kommunikation.
Bästa praxis är toocache `FabricServiceProxyFactory` så länge som möjligt.

## <a name="remoting-exception-handling"></a>Fjärrkommunikation undantagshantering
Alla hello remote undantagsfel från service API, skickas tillbaka toohello klienten som RuntimeException eller FabricException.

ServiceProxy hanterar alla redundans undantag för hello service partition som skapats för. Hello slutpunkter löser igen om det finns Failover Exceptions(Non-Transient Exceptions) och försöker hello-anrop med hello korrekt slutpunkt. Antal återförsök för redundans undantag är obegränsad.
Vid TransientExceptions, endast ett nytt försök hello-anrop.

Försök standardparametrarna är etablerar av [OperationRetrySettings]. (https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Användare kan konfigurera dessa värden genom att skicka OperationRetrySettings objektet tooServiceProxyFactory konstruktor.

## <a name="next-steps"></a>Nästa steg
* [Att säkra kommunikationen för Reliable Services](service-fabric-reliable-services-secure-communication.md)
