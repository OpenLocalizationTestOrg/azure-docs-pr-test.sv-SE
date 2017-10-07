---
title: "aaaDefinine och hantera tillstånd i Azure mikrotjänster | Microsoft Docs"
description: "Hur toodefine och hantera i Service Fabric-tjänstens tillstånd"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 4a24696da71753d0f343a86df3556b5b7c964834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-state"></a>Tillstånd för tjänsten
**Tjänsten tillstånd** refererar toohello i minnet eller på att en tjänst kräver toofunction diskdata. Den omfattar, till exempel hello datastrukturer och medlemsvariabler hello tjänsten läser och skriver toodo arbete. Beroende på hur hello-tjänsten är konstruerad, kan den också innehålla filer eller andra resurser som är lagrade på disken. Till exempel hello filer en-databas skulle använda toostore data och transaktionsloggarna.

Nu ska vi titta Kalkylatorn som en exempel-tjänst. En enkel kalkylator tjänst tar två tal och returnerar sina summan. Utför den här beräkningen innebär att inga Medlemsvariabler eller annan information.

Tänk dig nu hello samma Kalkylatorn, men det har beräknats med ytterligare en metod för att lagra och returnera hello sista summan. Den här tjänsten är nu tillståndskänsliga. Stateful innebär att den innehåller vissa tillstånd som skriver den toowhen den beräknar en ny summa och läser från när du begär tooreturn hello senaste beräknade summan.

I Azure Service Fabric kallas hello första tjänsten tillståndslösa tjänsten. hello andra tjänsten kallas en tillståndskänslig service.

## <a name="storing-service-state"></a>Spara tjänstens tillstånd
Tillstånd kan vara antingen externalized eller samordnad med hello-kod som manipulering hello tillstånd. Externalization i tillståndet normalt görs med hjälp av en extern databas eller andra data lagras att hello körs på olika datorer hello nätverket eller utanför processen på samma dator. I vårt exempel Kalkylatorn kan hello datalagret vara en SQL-databas eller en instans av Azure Table Store. Varje begäran toocompute hello summan utför en uppdatering på dessa data och begär toohello tjänsten tooreturn hello värde resulterar i hello aktuellt värde som hämtats från hello store. 

Tillståndet kan också vara samordnad med hello-kod som hanterar hello tillstånd. Tillståndskänsliga tjänster i Service Fabric skapas vanligtvis med hjälp av den här modellen. Service Fabric ger hello infrastruktur tooensure att det här tillståndet har hög tillgänglighet, konsekvent och varaktig och att hello services bygger det här sättet kan enkelt skala.

## <a name="next-steps"></a>Nästa steg
Mer information om Service Fabric-begrepp finns hello följande artiklar:

* [Tillgänglighet för Service Fabric-tjänster](service-fabric-availability-services.md)
* [Skalbarheten för Service Fabric-tjänster](service-fabric-concepts-scalability.md)
* [Partitionering Service Fabric-tjänster](service-fabric-concepts-partitioning.md)
* [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md)
