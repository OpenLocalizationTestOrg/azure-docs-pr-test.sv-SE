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
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a><span data-ttu-id="884ab-103">Hjälp för säker kommunikation för tjänster i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="884ab-103">Help secure communication for services in Azure Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="884ab-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="884ab-104">C# on Windows</span></span>](service-fabric-reliable-services-secure-communication.md)
> * [<span data-ttu-id="884ab-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="884ab-105">Java on Linux</span></span>](service-fabric-reliable-services-secure-communication-java.md)
>
>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a><span data-ttu-id="884ab-106">Skydda en tjänst när du använder tjänsten fjärrkommunikation</span><span class="sxs-lookup"><span data-stu-id="884ab-106">Help secure a service when you're using service remoting</span></span>
<span data-ttu-id="884ab-107">Vi ska använda en befintlig [exempel](service-fabric-reliable-services-communication-remoting-java.md) som förklarar hur tooset in fjärrstyrning för tillförlitlig tjänster.</span><span class="sxs-lookup"><span data-stu-id="884ab-107">We'll be using an existing [example](service-fabric-reliable-services-communication-remoting-java.md) that explains how tooset up remoting for reliable services.</span></span> <span data-ttu-id="884ab-108">toohelp skydda en tjänst när du använder tjänsten remoting, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="884ab-108">toohelp secure a service when you're using service remoting, follow these steps:</span></span>

1. <span data-ttu-id="884ab-109">Skapa ett gränssnitt `HelloWorldStateless`, som definierar hello-metoder som är tillgängliga för remote procedure call på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="884ab-109">Create an interface, `HelloWorldStateless`, that defines hello methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="884ab-110">Tjänsten använder `FabricTransportServiceRemotingListener`, som har deklarerats i hello `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` paketet.</span><span class="sxs-lookup"><span data-stu-id="884ab-110">Your service will use `FabricTransportServiceRemotingListener`, which is declared in hello `microsoft.serviceFabric.services.remoting.fabricTransport.runtime` package.</span></span> <span data-ttu-id="884ab-111">Det här är en `CommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="884ab-111">This is an `CommunicationListener` implementation that provides remoting capabilities.</span></span>

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
2. <span data-ttu-id="884ab-112">Lägga till lyssnarinställningarna och säkerhetsreferenser.</span><span class="sxs-lookup"><span data-stu-id="884ab-112">Add listener settings and security credentials.</span></span>

    <span data-ttu-id="884ab-113">Se till att hello-certifikat som du vill toouse toohelp säker service-kommunikation har installerats på alla hello-noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="884ab-113">Make sure that hello certificate that you want toouse toohelp secure your service communication is installed on all hello nodes in hello cluster.</span></span> <span data-ttu-id="884ab-114">Det finns två sätt att du kan ange inställningar för lyssnare och säkerhetsreferenser:</span><span class="sxs-lookup"><span data-stu-id="884ab-114">There are two ways that you can provide listener settings and security credentials:</span></span>

   1. <span data-ttu-id="884ab-115">Ge dem med hjälp av en [konfigurationspaketet](service-fabric-application-model.md):</span><span class="sxs-lookup"><span data-stu-id="884ab-115">Provide them by using a [config package](service-fabric-application-model.md):</span></span>

       <span data-ttu-id="884ab-116">Lägg till en `TransportSettings` -avsnittet i hello settings.xml.</span><span class="sxs-lookup"><span data-stu-id="884ab-116">Add a `TransportSettings` section in hello settings.xml file.</span></span>

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

       <span data-ttu-id="884ab-117">I det här fallet hello `createServiceInstanceListeners` metod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="884ab-117">In this case, hello `createServiceInstanceListeners` method will look like this:</span></span>

       ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this, FabricTransportRemotingListenerSettings.loadFrom(HelloWorldStatelessTransportSettings));
            }));
            return listeners;
        }
       ```

        <span data-ttu-id="884ab-118">Om du lägger till en `TransportSettings` -avsnittet i settings.xml hello utan valfritt prefix `FabricTransportListenerSettings` kommer att läsa in alla hello-inställningar från det här avsnittet som standard.</span><span class="sxs-lookup"><span data-stu-id="884ab-118">If you add a `TransportSettings` section in hello settings.xml file without any prefix, `FabricTransportListenerSettings` will load all hello settings from this section by default.</span></span>

        ```xml
        <!--"TransportSettings" section without any prefix.-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        <span data-ttu-id="884ab-119">I det här fallet hello `CreateServiceInstanceListeners` metod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="884ab-119">In this case, hello `CreateServiceInstanceListeners` method will look like this:</span></span>

        ```java
        protected List<ServiceInstanceListener> createServiceInstanceListeners() {
            ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
            listeners.add(new ServiceInstanceListener((context) -> {
                return new FabricTransportServiceRemotingListener(context,this);
            }));
            return listeners;
        }
       ```
3. <span data-ttu-id="884ab-120">När du anropar metoder på en skyddad tjänst med hjälp av hello fjärrkommunikation stacken, istället för att använda hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` klassen toocreate en Tjänstproxy, Använd `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="884ab-120">When you call methods on a secured service by using hello remoting stack, instead of using hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` class toocreate a service proxy, use `microsoft.serviceFabric.services.remoting.client.FabricServiceProxyFactory`.</span></span>

    <span data-ttu-id="884ab-121">Om hello klientkod körs som en del av en tjänst, kan du läsa in `FabricTransportSettings` från hello settings.xml fil.</span><span class="sxs-lookup"><span data-stu-id="884ab-121">If hello client code is running as part of a service, you can load `FabricTransportSettings` from hello settings.xml file.</span></span> <span data-ttu-id="884ab-122">Skapa en TransportSettings avsnitt liknande toohello Tjänstkod enligt tidigare.</span><span class="sxs-lookup"><span data-stu-id="884ab-122">Create a TransportSettings section that is similar toohello service code, as shown earlier.</span></span> <span data-ttu-id="884ab-123">Gör följande ändringar toohello klientkod hello:</span><span class="sxs-lookup"><span data-stu-id="884ab-123">Make hello following changes toohello client code:</span></span>

    ```java

    FabricServiceProxyFactory serviceProxyFactory = new FabricServiceProxyFactory(c -> {
            return new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.loadFrom("TransportPrefixTransportSettings"), null, null, null, null);
        }, null)

    HelloWorldStateless client = serviceProxyFactory.createServiceProxy(HelloWorldStateless.class,
        new URI("fabric:/MyApplication/MyHelloWorldService"));

    CompletableFuture<String> message = client.getHelloWorld();

    ```
