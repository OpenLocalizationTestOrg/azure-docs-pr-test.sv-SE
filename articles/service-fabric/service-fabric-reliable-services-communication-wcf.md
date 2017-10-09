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
# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF-baserad kommunikation stacken för Reliable Services
hello Reliable Services framework kan författarna toochoose hello kommunikation stack som de vill toouse för service-tjänsten. De kan ansluta hello kommunikation stack valfri via hello **ICommunicationListener** returnerades från hello [CreateServiceReplicaListeners eller CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) metoder. hello framework tillhandahåller en implementering av hello kommunikation stack baserat på hello Windows Communication Foundation (WCF) för tjänsten programutvecklare som vill toouse WCF-baserad kommunikation.

## <a name="wcf-communication-listener"></a>Lyssnare för WCF-kommunikation
hello WCF-specifika implementering av **ICommunicationListener** tillhandahålls av hello **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** klass.

Att ange Säg har vi ett servicekontrakt av typen`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Vi kan skapa en lyssnare för WCF-kommunikation i hello service hello följande sätt.

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

## <a name="writing-clients-for-hello-wcf-communication-stack"></a>Skrivning av klienter för hello WCF kommunikation stack
För att skriva klienter toocommunicate med tjänster med hjälp av WCF hello framework ger **WcfClientCommunicationFactory**, vilket är hello WCF-specifika implementering av [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md).

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

hello WCF kommunikationskanalen kan nås från hello **WcfCommunicationClient** skapas av hello **WcfCommunicationClientFactory**.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Klientkoden kan använda hello **WcfCommunicationClientFactory** tillsammans med hello **WcfCommunicationClient** som implementerar **ServicePartitionClient** toodetermine Hej tjänstslutpunkten och kommunicera med hello-tjänsten.

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
> hello standard ServicePartitionResolver förutsätter att hello-klienten körs i samma kluster som hello-tjänst. Om så är inte hello fallet, skapa ett ServicePartitionResolver-objekt och ange hello slutpunkter för klustret.
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Fjärrproceduranrop med Reliable Services fjärrkommunikation](service-fabric-reliable-services-communication-remoting.md)
* [Webb-API med OWIN i Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [Att säkra kommunikationen för Reliable Services](service-fabric-reliable-services-secure-communication.md)

