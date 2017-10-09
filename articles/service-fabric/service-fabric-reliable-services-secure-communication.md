---
title: "aaaHelp säker kommunikation för tjänster i Azure Service Fabric | Microsoft Docs"
description: "Översikt över hur toohelp säker kommunikation för tillförlitlig tjänster som körs i ett Azure Service Fabric-kluster."
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: vturecek
ms.assetid: fc129c1a-fbe4-4339-83ae-0e69a41654e0
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: suchiagicha
ms.openlocfilehash: 15201eb503322b17db329b319f1f42860b0f0c6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a><span data-ttu-id="92982-103">Hjälp för säker kommunikation för tjänster i Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="92982-103">Help secure communication for services in Azure Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="92982-104">C# i Windows</span><span class="sxs-lookup"><span data-stu-id="92982-104">C# on Windows</span></span>](service-fabric-reliable-services-secure-communication.md)
> * [<span data-ttu-id="92982-105">Java i Linux</span><span class="sxs-lookup"><span data-stu-id="92982-105">Java on Linux</span></span>](service-fabric-reliable-services-secure-communication-java.md)
>
>

<span data-ttu-id="92982-106">Säkerhet är hello mest viktiga aspekter av kommunikation.</span><span class="sxs-lookup"><span data-stu-id="92982-106">Security is one of hello most important aspects of communication.</span></span> <span data-ttu-id="92982-107">hello Reliable Services-programmet innehåller några fördefinierade kommunikation stackar och verktyg som du kan använda tooimprove säkerhet.</span><span class="sxs-lookup"><span data-stu-id="92982-107">hello Reliable Services application framework provides a few prebuilt communication stacks and tools that you can use tooimprove security.</span></span> <span data-ttu-id="92982-108">Den här artikeln handlar om hur tooimprove säkerhet när du använder tjänsten fjärrkommunikation och hello Windows Communication Foundation (WCF) kommunikation stacken.</span><span class="sxs-lookup"><span data-stu-id="92982-108">This article talks about how tooimprove security when you're using service remoting and hello Windows Communication Foundation (WCF) communication stack.</span></span>

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a><span data-ttu-id="92982-109">Skydda en tjänst när du använder tjänsten fjärrkommunikation</span><span class="sxs-lookup"><span data-stu-id="92982-109">Help secure a service when you're using service remoting</span></span>
<span data-ttu-id="92982-110">Vi använder en befintlig [exempel](service-fabric-reliable-services-communication-remoting.md) som förklarar hur tooset in fjärrstyrning för tillförlitlig tjänster.</span><span class="sxs-lookup"><span data-stu-id="92982-110">We are using an existing [example](service-fabric-reliable-services-communication-remoting.md) that explains how tooset up remoting for reliable services.</span></span> <span data-ttu-id="92982-111">toohelp skydda en tjänst när du använder tjänsten remoting, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="92982-111">toohelp secure a service when you're using service remoting, follow these steps:</span></span>

1. <span data-ttu-id="92982-112">Skapa ett gränssnitt `IHelloWorldStateful`, som definierar hello-metoder som är tillgängliga för remote procedure call på din tjänst.</span><span class="sxs-lookup"><span data-stu-id="92982-112">Create an interface, `IHelloWorldStateful`, that defines hello methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="92982-113">Tjänsten använder `FabricTransportServiceRemotingListener`, som har deklarerats i hello `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` namnområde.</span><span class="sxs-lookup"><span data-stu-id="92982-113">Your service will use `FabricTransportServiceRemotingListener`, which is declared in hello `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` namespace.</span></span> <span data-ttu-id="92982-114">Det här är en `ICommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation.</span><span class="sxs-lookup"><span data-stu-id="92982-114">This is an `ICommunicationListener` implementation that provides remoting capabilities.</span></span>

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```
2. <span data-ttu-id="92982-115">Lägga till lyssnarinställningarna och säkerhetsreferenser.</span><span class="sxs-lookup"><span data-stu-id="92982-115">Add listener settings and security credentials.</span></span>

    <span data-ttu-id="92982-116">Se till att hello-certifikat som du vill toouse toohelp säker service-kommunikation har installerats på alla hello-noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="92982-116">Make sure that hello certificate that you want toouse toohelp secure your service communication is installed on all hello nodes in hello cluster.</span></span> <span data-ttu-id="92982-117">Det finns två sätt att du kan ange inställningar för lyssnare och säkerhetsreferenser:</span><span class="sxs-lookup"><span data-stu-id="92982-117">There are two ways that you can provide listener settings and security credentials:</span></span>

   1. <span data-ttu-id="92982-118">Ge dem direkt i hello service-koden:</span><span class="sxs-lookup"><span data-stu-id="92982-118">Provide them directly in hello service code:</span></span>

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           FabricTransportRemotingListenerSettings  listenerSettings = new FabricTransportRemotingListenerSettings
           {
               MaxMessageSize = 10000000,
               SecurityCredentials = GetSecurityCredentials()
           };
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
           };
       }

       private static SecurityCredentials GetSecurityCredentials()
       {
           // Provide certificate details.
           var x509Credentials = new X509Credentials
           {
               FindType = X509FindType.FindByThumbprint,
               FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
               StoreLocation = StoreLocation.LocalMachine,
               StoreName = "My",
               ProtectionLevel = ProtectionLevel.EncryptAndSign
           };
           x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
           x509Credentials.RemoteCertThumbprints.Add("9FEF3950642138446CC364A396E1E881DB76B483");
           return x509Credentials;
       }
       ```
   2. <span data-ttu-id="92982-119">Ge dem med hjälp av en [konfigurationspaketet](service-fabric-application-model.md):</span><span class="sxs-lookup"><span data-stu-id="92982-119">Provide them by using a [config package](service-fabric-application-model.md):</span></span>

       <span data-ttu-id="92982-120">Lägg till en `TransportSettings` -avsnittet i hello settings.xml.</span><span class="sxs-lookup"><span data-stu-id="92982-120">Add a `TransportSettings` section in hello settings.xml file.</span></span>

       ```xml
       <Section Name="HelloWorldStatefulTransportSettings">
           <Parameter Name="MaxMessageSize" Value="10000000" />
           <Parameter Name="SecurityCredentialsType" Value="X509" />
           <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
           <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
           <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
           <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
           <Parameter Name="CertificateStoreName" Value="My" />
           <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
           <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
       </Section>
       ```

       <span data-ttu-id="92982-121">I det här fallet hello `CreateServiceReplicaListeners` metod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="92982-121">In this case, hello `CreateServiceReplicaListeners` method will look like this:</span></span>

       ```csharp
       protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
       {
           return new[]
           {
               new ServiceReplicaListener(
                   (context) => new FabricTransportServiceRemotingListener(
                       context,this,FabricTransportRemotingListenerSettings .LoadFrom("HelloWorldStatefulTransportSettings")))
           };
       }
       ```

        <span data-ttu-id="92982-122">Om du lägger till en `TransportSettings` i hello settings.xml filen `FabricTransportRemotingListenerSettings ` kommer att läsa in alla hello-inställningar från det här avsnittet som standard.</span><span class="sxs-lookup"><span data-stu-id="92982-122">If you add a `TransportSettings` section in hello settings.xml file , `FabricTransportRemotingListenerSettings ` will load all hello settings from this section by default.</span></span>

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        <span data-ttu-id="92982-123">I det här fallet hello `CreateServiceReplicaListeners` metod ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="92982-123">In this case, hello `CreateServiceReplicaListeners` method will look like this:</span></span>

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                return new[]{
                        new ServiceReplicaListener(
                            (context) => new FabricTransportServiceRemotingListener(context,this))};
            };
        }
        ```
3. <span data-ttu-id="92982-124">När du anropar metoder på en skyddad tjänst med hjälp av hello fjärrkommunikation stacken, istället för att använda hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klassen toocreate en Tjänstproxy, Använd `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="92982-124">When you call methods on a secured service by using hello remoting stack, instead of using hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` class toocreate a service proxy, use `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`.</span></span> <span data-ttu-id="92982-125">Skicka in `FabricTransportRemotingSettings`, som innehåller `SecurityCredentials`.</span><span class="sxs-lookup"><span data-stu-id="92982-125">Pass in `FabricTransportRemotingSettings`, which contains `SecurityCredentials`.</span></span>

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "9FEF3950642138446CC364A396E1E881DB76B483",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
    x509Credentials.RemoteCertThumbprints.Add("4FEF3950642138446CC364A396E1E881DB76B48C");

    FabricTransportRemotingSettings transportSettings = new FabricTransportRemotingSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    <span data-ttu-id="92982-126">Om hello klientkod körs som en del av en tjänst, kan du läsa in `FabricTransportRemotingSettings` från hello settings.xml fil.</span><span class="sxs-lookup"><span data-stu-id="92982-126">If hello client code is running as part of a service, you can load `FabricTransportRemotingSettings` from hello settings.xml file.</span></span> <span data-ttu-id="92982-127">Skapa en HelloWorldClientTransportSettings avsnitt liknande toohello Tjänstkod enligt tidigare.</span><span class="sxs-lookup"><span data-stu-id="92982-127">Create a HelloWorldClientTransportSettings section that is similar toohello service code, as shown earlier.</span></span> <span data-ttu-id="92982-128">Gör följande ändringar toohello klientkod hello:</span><span class="sxs-lookup"><span data-stu-id="92982-128">Make hello following changes toohello client code:</span></span>

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    <span data-ttu-id="92982-129">Om hello klienten inte körs som en del av en tjänst, du kan skapa en client_name.settings.xml-fil i hello samma plats där hello client_name.exe.</span><span class="sxs-lookup"><span data-stu-id="92982-129">If hello client is not running as part of a service, you can create a client_name.settings.xml file in hello same location where hello client_name.exe is.</span></span> <span data-ttu-id="92982-130">Skapa sedan en TransportSettings avsnitt i filen.</span><span class="sxs-lookup"><span data-stu-id="92982-130">Then create a TransportSettings section in that file.</span></span>

    <span data-ttu-id="92982-131">Liknande toohello tjänsten, om du lägger till en `TransportSettings` avsnitt i klienten settings.xml/client_name.settings.xml `FabricTransportRemotingSettings` läser in alla hello-inställningar från det här avsnittet som standard.</span><span class="sxs-lookup"><span data-stu-id="92982-131">Similar toohello service, if you add a `TransportSettings` section in client settings.xml/client_name.settings.xml, `FabricTransportRemotingSettings` loads all hello settings from this section by default.</span></span>

    <span data-ttu-id="92982-132">I så fall förenklas hello tidigare kod ytterligare:</span><span class="sxs-lookup"><span data-stu-id="92982-132">In that case, hello earlier code is even further simplified:</span></span>  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a><span data-ttu-id="92982-133">Skydda en tjänst när du använder en WCF-baserad kommunikation stack</span><span class="sxs-lookup"><span data-stu-id="92982-133">Help secure a service when you're using a WCF-based communication stack</span></span>
<span data-ttu-id="92982-134">Vi använder en befintlig [exempel](service-fabric-reliable-services-communication-wcf.md) som förklarar hur tooset upp en WCF-baserad kommunikation stacken för tillförlitlig tjänster.</span><span class="sxs-lookup"><span data-stu-id="92982-134">We are using an existing [example](service-fabric-reliable-services-communication-wcf.md) that explains how tooset up a WCF-based communication stack for reliable services.</span></span> <span data-ttu-id="92982-135">toohelp skydda en tjänst när du använder en WCF-baserad kommunikation stack, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="92982-135">toohelp secure a service when you're using a WCF-based communication stack, follow these steps:</span></span>

1. <span data-ttu-id="92982-136">För hello service måste toohelp säker hello WCF kommunikation lyssnare (`WcfCommunicationListener`) som du skapar.</span><span class="sxs-lookup"><span data-stu-id="92982-136">For hello service, you need toohelp secure hello WCF communication listener (`WcfCommunicationListener`) that you create.</span></span> <span data-ttu-id="92982-137">toodo, ändra hello `CreateServiceReplicaListeners` metod.</span><span class="sxs-lookup"><span data-stu-id="92982-137">toodo this, modify hello `CreateServiceReplicaListeners` method.</span></span>

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in hello ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```
2. <span data-ttu-id="92982-138">Hello-klienten hello `WcfCommunicationClient` klass som har skapats i hello tidigare [exempel](service-fabric-reliable-services-communication-wcf.md) förblir oförändrad.</span><span class="sxs-lookup"><span data-stu-id="92982-138">In hello client, hello `WcfCommunicationClient` class that was created in hello previous [example](service-fabric-reliable-services-communication-wcf.md) remains unchanged.</span></span> <span data-ttu-id="92982-139">Men du måste toooverride hello `CreateClientAsync` metod för `WcfCommunicationClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="92982-139">But you need toooverride hello `CreateClientAsync` method of `WcfCommunicationClientFactory`:</span></span>

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details toohello ChannelFactory credentials.
            // These credentials will be used by hello clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    <span data-ttu-id="92982-140">Använd `SecureWcfCommunicationClientFactory` toocreate en WCF-klient för kommunikation (`WcfCommunicationClient`).</span><span class="sxs-lookup"><span data-stu-id="92982-140">Use `SecureWcfCommunicationClientFactory` toocreate a WCF communication client (`WcfCommunicationClient`).</span></span> <span data-ttu-id="92982-141">Använd hello klienten tooinvoke service metoder.</span><span class="sxs-lookup"><span data-stu-id="92982-141">Use hello client tooinvoke service methods.</span></span>

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a><span data-ttu-id="92982-142">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="92982-142">Next steps</span></span>
* [<span data-ttu-id="92982-143">Webb-API med OWIN i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="92982-143">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
