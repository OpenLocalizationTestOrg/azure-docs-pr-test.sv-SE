---
title: aaaReliable WCF Services kommunikation stack | Microsoft Docs
description: "hello inbyggda WCF kommunikation stacken i Service Fabric ger-klienttjänsten WCF-kommunikation för Reliable Services."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 75516e1e-ee57-4bc7-95fe-71ec42d452b2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/07/2017
ms.author: bharatn
ms.openlocfilehash: 7feebef4d46a6ae66d05129f47f9b5911e82aec9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="wcf-based-communication-stack-for-reliable-services"></a><span data-ttu-id="0f146-103">WCF-baserad kommunikation stacken för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="0f146-103">WCF-based communication stack for Reliable Services</span></span>
<span data-ttu-id="0f146-104">hello Reliable Services framework kan författarna toochoose hello kommunikation stack som de vill toouse för service-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0f146-104">hello Reliable Services framework allows service authors toochoose hello communication stack that they want toouse for their service.</span></span> <span data-ttu-id="0f146-105">De kan ansluta hello kommunikation stack valfri via hello **ICommunicationListener** returnerades från hello [CreateServiceReplicaListeners eller CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) metoder.</span><span class="sxs-lookup"><span data-stu-id="0f146-105">They can plug in hello communication stack of their choice via hello **ICommunicationListener** returned from hello [CreateServiceReplicaListeners or CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) methods.</span></span> <span data-ttu-id="0f146-106">hello framework tillhandahåller en implementering av hello kommunikation stack baserat på hello Windows Communication Foundation (WCF) för tjänsten programutvecklare som vill toouse WCF-baserad kommunikation.</span><span class="sxs-lookup"><span data-stu-id="0f146-106">hello framework provides an implementation of hello communication stack based on hello Windows Communication Foundation (WCF) for service authors who want toouse WCF-based communication.</span></span>

## <a name="wcf-communication-listener"></a><span data-ttu-id="0f146-107">Lyssnare för WCF-kommunikation</span><span class="sxs-lookup"><span data-stu-id="0f146-107">WCF Communication Listener</span></span>
<span data-ttu-id="0f146-108">hello WCF-specifika implementering av **ICommunicationListener** tillhandahålls av hello **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** klass.</span><span class="sxs-lookup"><span data-stu-id="0f146-108">hello WCF-specific implementation of **ICommunicationListener** is provided by hello **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** class.</span></span>

<span data-ttu-id="0f146-109">Att ange Säg har vi ett servicekontrakt av typen`ICalculator`</span><span class="sxs-lookup"><span data-stu-id="0f146-109">Lest say we have a service contract of type `ICalculator`</span></span>

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

<span data-ttu-id="0f146-110">Vi kan skapa en lyssnare för WCF-kommunikation i hello service hello följande sätt.</span><span class="sxs-lookup"><span data-stu-id="0f146-110">We can create a WCF communication listener in hello service hello following manner.</span></span>

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // hello name of hello endpoint configured in hello ServiceManifest under hello Endpoints section
            // that identifies hello endpoint that hello WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate hello binding information that you want hello service toouse.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-hello-wcf-communication-stack"></a><span data-ttu-id="0f146-111">Skrivning av klienter för hello WCF kommunikation stack</span><span class="sxs-lookup"><span data-stu-id="0f146-111">Writing clients for hello WCF communication stack</span></span>
<span data-ttu-id="0f146-112">För att skriva klienter toocommunicate med tjänster med hjälp av WCF hello framework ger **WcfClientCommunicationFactory**, vilket är hello WCF-specifika implementering av [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="0f146-112">For writing clients toocommunicate with services by using WCF, hello framework provides **WcfClientCommunicationFactory**, which is hello WCF-specific implementation of [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).</span></span>

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

<span data-ttu-id="0f146-113">hello WCF kommunikationskanalen kan nås från hello **WcfCommunicationClient** skapas av hello **WcfCommunicationClientFactory**.</span><span class="sxs-lookup"><span data-stu-id="0f146-113">hello WCF communication channel can be accessed from hello **WcfCommunicationClient** created by hello **WcfCommunicationClientFactory**.</span></span>

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

<span data-ttu-id="0f146-114">Klientkoden kan använda hello **WcfCommunicationClientFactory** tillsammans med hello **WcfCommunicationClient** som implementerar **ServicePartitionClient** toodetermine Hej tjänstslutpunkten och kommunicera med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0f146-114">Client code can use hello **WcfCommunicationClientFactory** along with hello **WcfCommunicationClient** which implements **ServicePartitionClient** toodetermine hello service endpoint and communicate with hello service.</span></span>

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with hello ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call hello service tooperform hello operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
> [!NOTE]
> <span data-ttu-id="0f146-115">hello standard ServicePartitionResolver förutsätter att hello-klienten körs i samma kluster som hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="0f146-115">hello default ServicePartitionResolver assumes that hello client is running in same cluster as hello service.</span></span> <span data-ttu-id="0f146-116">Om så är inte hello fallet, skapa ett ServicePartitionResolver-objekt och ange hello slutpunkter för klustret.</span><span class="sxs-lookup"><span data-stu-id="0f146-116">If that is not hello case, create a ServicePartitionResolver object and pass in hello cluster connection endpoints.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="0f146-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f146-117">Next steps</span></span>
* [<span data-ttu-id="0f146-118">Fjärrproceduranrop med Reliable Services fjärrkommunikation</span><span class="sxs-lookup"><span data-stu-id="0f146-118">Remote procedure call with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="0f146-119">Webb-API med OWIN i Reliable Services</span><span class="sxs-lookup"><span data-stu-id="0f146-119">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="0f146-120">Att säkra kommunikationen för Reliable Services</span><span class="sxs-lookup"><span data-stu-id="0f146-120">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)

