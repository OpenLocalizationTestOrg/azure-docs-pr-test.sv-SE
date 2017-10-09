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
# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Hjälp för säker kommunikation för tjänster i Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# i Windows](service-fabric-reliable-services-secure-communication.md)
> * [Java i Linux](service-fabric-reliable-services-secure-communication-java.md)
>
>

Säkerhet är hello mest viktiga aspekter av kommunikation. hello Reliable Services-programmet innehåller några fördefinierade kommunikation stackar och verktyg som du kan använda tooimprove säkerhet. Den här artikeln handlar om hur tooimprove säkerhet när du använder tjänsten fjärrkommunikation och hello Windows Communication Foundation (WCF) kommunikation stacken.

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Skydda en tjänst när du använder tjänsten fjärrkommunikation
Vi använder en befintlig [exempel](service-fabric-reliable-services-communication-remoting.md) som förklarar hur tooset in fjärrstyrning för tillförlitlig tjänster. toohelp skydda en tjänst när du använder tjänsten remoting, gör du följande:

1. Skapa ett gränssnitt `IHelloWorldStateful`, som definierar hello-metoder som är tillgängliga för remote procedure call på din tjänst. Tjänsten använder `FabricTransportServiceRemotingListener`, som har deklarerats i hello `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` namnområde. Det här är en `ICommunicationListener` implementering som tillhandahåller funktioner för fjärrkommunikation.

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
2. Lägga till lyssnarinställningarna och säkerhetsreferenser.

    Se till att hello-certifikat som du vill toouse toohelp säker service-kommunikation har installerats på alla hello-noder i klustret hello. Det finns två sätt att du kan ange inställningar för lyssnare och säkerhetsreferenser:

   1. Ge dem direkt i hello service-koden:

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
   2. Ge dem med hjälp av en [konfigurationspaketet](service-fabric-application-model.md):

       Lägg till en `TransportSettings` -avsnittet i hello settings.xml.

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

       I det här fallet hello `CreateServiceReplicaListeners` metod ser ut så här:

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

        Om du lägger till en `TransportSettings` i hello settings.xml filen `FabricTransportRemotingListenerSettings ` kommer att läsa in alla hello-inställningar från det här avsnittet som standard.

        ```xml
        <!--"TransportSettings" section .-->
        <Section Name="TransportSettings">
            ...
        </Section>
        ```
        I det här fallet hello `CreateServiceReplicaListeners` metod ser ut så här:

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
3. När du anropar metoder på en skyddad tjänst med hjälp av hello fjärrkommunikation stacken, istället för att använda hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` klassen toocreate en Tjänstproxy, Använd `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Skicka in `FabricTransportRemotingSettings`, som innehåller `SecurityCredentials`.

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

    Om hello klientkod körs som en del av en tjänst, kan du läsa in `FabricTransportRemotingSettings` från hello settings.xml fil. Skapa en HelloWorldClientTransportSettings avsnitt liknande toohello Tjänstkod enligt tidigare. Gör följande ändringar toohello klientkod hello:

    ```csharp
    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportRemotingSettings.LoadFrom("HelloWorldClientTransportSettings")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Om hello klienten inte körs som en del av en tjänst, du kan skapa en client_name.settings.xml-fil i hello samma plats där hello client_name.exe. Skapa sedan en TransportSettings avsnitt i filen.

    Liknande toohello tjänsten, om du lägger till en `TransportSettings` avsnitt i klienten settings.xml/client_name.settings.xml `FabricTransportRemotingSettings` läser in alla hello-inställningar från det här avsnittet som standard.

    I så fall förenklas hello tidigare kod ytterligare:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Skydda en tjänst när du använder en WCF-baserad kommunikation stack
Vi använder en befintlig [exempel](service-fabric-reliable-services-communication-wcf.md) som förklarar hur tooset upp en WCF-baserad kommunikation stacken för tillförlitlig tjänster. toohelp skydda en tjänst när du använder en WCF-baserad kommunikation stack, gör du följande:

1. För hello service måste toohelp säker hello WCF kommunikation lyssnare (`WcfCommunicationListener`) som du skapar. toodo, ändra hello `CreateServiceReplicaListeners` metod.

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
2. Hello-klienten hello `WcfCommunicationClient` klass som har skapats i hello tidigare [exempel](service-fabric-reliable-services-communication-wcf.md) förblir oförändrad. Men du måste toooverride hello `CreateClientAsync` metod för `WcfCommunicationClientFactory`:

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

    Använd `SecureWcfCommunicationClientFactory` toocreate en WCF-klient för kommunikation (`WcfCommunicationClient`). Använd hello klienten tooinvoke service metoder.

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

## <a name="next-steps"></a>Nästa steg
* [Webb-API med OWIN i Reliable Services](service-fabric-reliable-services-communication-webapi.md)
