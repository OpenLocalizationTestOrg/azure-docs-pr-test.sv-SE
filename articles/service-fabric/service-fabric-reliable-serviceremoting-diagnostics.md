---
title: "aaaAzure ServiceFabric diagnostik och övervakning | Microsoft Docs"
description: "Den här artikeln beskriver hello prestanda övervakningsfunktionerna i hello tillförlitliga ServiceRemoting för Service Fabric runtime som prestandaräknare som sänds av den."
services: service-fabric
documentationcenter: .net
author: suchiagicha
manager: timlt
editor: suchiagicha
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: suchiagicha
ms.openlocfilehash: 64db9a890bd59a1326e587d14b89c007b71a9059
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-service-remoting"></a>Diagnostik- och prestandaövervakning för tillförlitlig tjänst fjärrkommunikation
hello tillförlitliga ServiceRemoting runtime avger [prestandaräknare](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Dessa ger insikter om hur hello ServiceRemoting fungerar och hjälp med felsökning och övervakning av programprestanda.


## <a name="performance-counters"></a>Prestandaräknare
hello tillförlitliga ServiceRemoting runtime definierar hello följande prestandaräknarkategorier:

| Kategori | Beskrivning |
| --- | --- |
| Fabric-tjänsten |Räknare för specifika tooAzure fjärrkommunikation med Service Fabric-tjänsten, till exempel Genomsnittlig tid tooprocess begäran |
| Metod för Service Fabric-tjänst |Räknare specifika toomethods implementeras av Service Fabric-fjärranslutningstjänsten, till exempel hur ofta en Servicemetoden har anropats |

Varje hello föregående kategorier har en eller flera räknare.

Hej [Windows Prestandaövervakaren](https://technet.microsoft.com/library/cc749249.aspx) program som är tillgängligt som standard i hello Windows operativsystem kan vara används toocollect och visa information om prestandaräknare. [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) är ett annat alternativ för att samla in prestandaräknardata och överföra den tooAzure tabeller.

### <a name="performance-counter-instance-names"></a>Namn på prestandaräknarinstanser
Ett kluster med ett stort antal ServiceRemoting tjänster eller partitioner har ett stort antal instanser för räknaren av prestanda. Hej prestandaräknarinstans namn hjälper dig identifiera hello specifik partition och metod för tjänst (om tillämpligt) som hello prestandaräknarinstans är associerad med.

#### <a name="service-fabric-service-category"></a>Fabric-tjänsten kategori
För hello kategori `Service Fabric Service`, hello namn på prestandaräknarinstanser finns i hello följande format:

`ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*ServiceFabricPartitionID* är hello strängrepresentation av hello Service Fabric partitions-ID som hello prestandaräknarinstans är associerad med. hello partitions-ID är ett GUID och dess strängrepresentation genereras via hello [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) metod med Formatspecificeraren ”D”.

*ServiceReplicaOrInstanceId* är hello strängrepresentation av hello Service Fabric repliken/instans-ID som hello prestandaräknarinstans är associerad med.

*ServiceRuntimeInternalID* är hello strängrepresentation av ett 64-bitars heltal som genereras av hello Fabric Service runtime för intern användning. Detta ingår i hello prestandaräknarinstans namn tooensure dess unika och undvika konflikter med andra namn på prestandaräknarinstanser. Användarna bör inte försöka toointerpret den här delen av hello namn på prestandaräknare instans.

hello följande är ett exempel på ett räknaren instansnamn för en räknare som tillhör toohello `Service Fabric Service` kategori:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046_5008379932`

I föregående exempel hello `2740af29-78aa-44bc-a20b-7e60fb783264` är hello strängrepresentation av hello Service Fabric partitions-ID, `635650083799324046` är strängrepresentation av repliken/InstanceId och `5008379932` är hello 64-bitars-ID som genereras för hello runtime internt Använd.

#### <a name="service-fabric-service-method-category"></a>Service Fabric-tjänsten metoden kategori
För hello kategori `Service Fabric Service Method`, hello namn på prestandaräknarinstanser finns i hello följande format:

`MethodName_ServiceRuntimeMethodId_ServiceFabricPartitionID_ServiceReplicaOrInstanceId_ServiceRuntimeInternalID`

*MethodName* hello metodens namn hello-tjänsten som hello prestandaräknarinstans associeras med. hello format för metodnamn hello bestäms baserat på vissa logiken i hello Fabric Service runtime som balanserar hello läsbarhet hello namn med begränsningar på hello högst hello namn på prestandaräknarinstanser i Windows.

*ServiceRuntimeMethodId* är hello strängrepresentation av en 32-bitars heltal som genereras av hello Fabric Service runtime för intern användning. Detta ingår i hello prestandaräknarinstans namn tooensure dess unika och undvika konflikter med andra namn på prestandaräknarinstanser. Användarna bör inte försöka toointerpret den här delen av hello namn på prestandaräknare instans.

*ServiceFabricPartitionID* är hello strängrepresentation av hello Service Fabric partitions-ID som hello prestandaräknarinstans är associerad med. hello partitions-ID är ett GUID och dess strängrepresentation genereras via hello [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) metod med Formatspecificeraren ”D”.

*ServiceReplicaOrInstanceId* är hello strängrepresentation av hello Service Fabric repliken/instans-ID som hello prestandaräknarinstans är associerad med.

*ServiceRuntimeInternalID* är hello strängrepresentation av ett 64-bitars heltal som genereras av hello Fabric Service runtime för intern användning. Detta ingår i hello prestandaräknarinstans namn tooensure dess unika och undvika konflikter med andra namn på prestandaräknarinstanser. Användarna bör inte försöka toointerpret den här delen av hello namn på prestandaräknare instans.

hello följande är ett exempel på ett räknaren instansnamn för en räknare som tillhör toohello `Service Fabric Service Method` kategori:

`ivoicemailboxservice.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486_5008380`

I föregående exempel hello `ivoicemailboxservice.leavemessageasync` är hello metodnamnet `2` är hello 32-bitars-ID som genererades för hello runtime internt använder `89383d32-e57e-4a9b-a6ad-57c6792aa521` är hello strängrepresentation av hello Service Fabric partitions-ID,`635650083804480486` hello sträng representation av hello Service Fabric-replik-/ instans-ID och `5008380` är hello 64-bitars-ID som genererades för hello runtime internt använder.

## <a name="list-of-performance-counters"></a>Lista över prestandaräknare
### <a name="service-method-performance-counters"></a>Prestandaräknare för tjänsten metod

hello tillförlitlig tjänst runtime publicerar hello efter Prestandaräknare relaterade toohello Webbtjänstmetoder.

| Kategorinamn | Räknarens namn | Beskrivning |
| --- | --- | --- |
| Metod för Service Fabric-tjänst |Anrop/sek |Antalet gånger som hello service-metoden har anropats per sekund |
| Metod för Service Fabric-tjänst |Genomsnittlig tid i millisekunder per anrop |Tid tooexecute hello webbtjänstmetoden i millisekunder |
| Metod för Service Fabric-tjänst |Undantag/sek |Antalet gånger som hello webbtjänstmetoden utlöste ett undantag per sekund |

### <a name="service-request-processing-performance-counters"></a>Prestandaräknare för tjänsten begäran bearbetning
När en klient anropar en metod via en proxy webbtjänstobjektet, resulterar det i ett meddelande om begäran som skickas via hello nätverkstjänsten toohello fjärrkommunikation. hello-tjänsten bearbetar begäran hello-meddelande och skickar en svar tillbaka toohello klient. hello tillförlitliga ServiceRemoting runtime publicerar hello efter Prestandaräknare relaterade tooservice behandling.

| Kategorinamn | Räknarens namn | Beskrivning |
| --- | --- | --- |
| Fabric-tjänsten |Antal väntande förfrågningar |Antalet begäranden som bearbetas i hello-tjänsten |
| Fabric-tjänsten |Genomsnittlig tid i millisekunder per begäran |Tid (i millisekunder) som hello service tooprocess en begäran |
| Fabric-tjänsten |Genomsnittlig tid i millisekunder för deserialiseringsbegäran |Tid vidtas (i millisekunder) toodeserialize service begärandemeddelandet när den tas emot med hello-tjänsten |
| Fabric-tjänsten |Genomsnittlig tid i millisekunder för serialiseringssvar |Svarsmeddelandet för tid vidtas (i millisekunder) tooserialize hello-tjänsten på hello-tjänsten innan hello svar skickas toohello klienten |

## <a name="next-steps"></a>Nästa steg
* [Exempelkod](https://github.com/Azure/servicefabric-samples)
* [EventSource providrar på förhandsgranskning](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
