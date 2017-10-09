---
title: "aaaHelp säker kommunikation för tjänster i Azure Service Fabric | Microsoft Docs"
description: "Översikt över hur toohelp säker kommunikation för tillförlitlig tjänster som körs i ett Azure Service Fabric-kluster."
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
ms.openlocfilehash: 14db54d50c35478c1f2c156de0dba36f1427c8cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Hjälp för säker kommunikation för tjänster i Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# i Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java i Linux](service-fabric-reliable-services-secure-communication-java.md)
>
>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Skydda en tjänst när du använder tjänsten fjärrkommunikation
Vi ska använda en befintlig [exempel](service-fabric-reliable-services-communication-remoting-java.md) som förklarar hur tooset in fjärrstyrning för tillförlitlig tjänster. toohelp skydda en tjänst när du använder tjänsten remoting, gör du följande:

1. Skapa ett gränssnitt `HelloWorldStateless`, som definierar hello-metoder som är tillgängliga för remote procedure call på din tjänst. Tjänsten använder `FabricTransportServiceRemotingListener`, som har deklarerats i hello `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` paketet. Det här är en `CommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation.

    ```java
    public interface HelloWorldStateless extends Service {
        CompletableFuture<String> getHelloWorld();
    }

    class HelloWorldStatelessImpl extends StatelessService implements HelloWorldStateless {
        @Override
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
        return listeners;
        }

        public CompletableFuture<String> getHelloWorld() {
            return CompletableFuture.completedFuture("Hello World!");
        }
    }
    ```
2. Lägga till lyssnarinställningarna och säkerhetsreferenser.

    Se till att hello-certifikat som du vill toouse toohelp säker service-kommunikation har installerats på alla hello-noder i klustret hello. Det finns två sätt att du kan ange inställningar för lyssnare och säkerhetsreferenser:

   1. Ge dem med hjälp av en [konfigurationspaketet](service-fabric-application-model.md):

       Lägg till en `TransportSettings` -avsnittet i hello settings.xml.

       ```xml
       <!--Section name should always end with "TransportSettings".-->
       <!--Here we are using a prefix "HelloWorldStateless".-->
        <Section Name="HelloWorldStatelessTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509_2" />
            <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
            <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
        </Section>

       ```

       I det här fallet hello `createServiceInstanceListeners` metod ser ut så här:

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        Om du lägger till en `TransportSettings` -avsnittet i settings.xml hello utan valfritt prefix `FabricTransportListenerSettings` kommer att läsa in alla hello-inställningar från det här avsnittet som standard.

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        I det här fallet hello `CreateServiceInstanceListeners` metod ser ut så här:

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. När du anropar metoder på en skyddad tjänst med hjälp av hello fjärrkommunikation stacken, istället för att använda hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klassen toocreate en Tjänstproxy, Använd `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.

    Om hello klientkod körs som en del av en tjänst, kan du läsa in `FabricTransportSettings` från hello settings.xml fil. Skapa en TransportSettings avsnitt liknande toohello Tjänstkod enligt tidigare. Gör följande ändringar toohello klientkod hello:

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
