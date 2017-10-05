---
title: "Tillförlitlig service-arkitektur | Microsoft Docs"
description: "Översikt över arkitekturen tillförlitlig tjänst för tillståndskänsliga och tillståndslösa tjänster"
services: service-fabric
documentationcenter: .net
author: AlanWarwick
manager: timlt
editor: vturecek
ms.assetid: af002ae6-7f6d-4769-b049-82aa1ba0891b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/30/2016
ms.author: alanwar
redirect_url: /azure/service-fabric/service-fabric-reliable-services-introduction
ms.openlocfilehash: a00a16945356b9731485554e06df46528b5c7bb2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Arkitektur för tillståndskänsliga och tillståndslösa Reliable Services
En tillförlitlig tjänst för Azure Service Fabric kanske tillståndskänslig eller tillståndslösa. Varje typ av tjänst körs inom en viss arkitektur. Dessa arkitekturer beskrivs i den här artikeln.
Finns det [tillförlitliga översikt över](service-fabric-reliable-services-introduction.md) för mer information om skillnaderna mellan tillståndskänsliga och tillståndslösa tjänster.

## <a name="stateful-reliable-services"></a>Tillståndskänsliga Reliable Services
### <a name="architecture-of-a-stateful-service"></a>Arkitekturen för en tillståndskänslig service
![Arkitekturdiagram för en tillståndskänslig tjänst](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Tillståndskänsliga tillförlitlig tjänst
En tillståndskänslig tillförlitlig tjänst kan härledas från den StatefulService eller StatefulServiceBase klassen. Båda dessa basklasser som tillhandahålls av Service Fabric. De tillhandahåller olika nivåer av support och abstraktion för tjänsten tillståndskänslig att samverka med Service Fabric - och ingår som en tjänst i Service Fabric-kluster.

StatefulService härleds från StatefulServiceBase. StatefulServiceBase ger bättre flexibilitet för tjänster, men kräver mer förståelse av innehållet i Service Fabric.
Finns det [översikt över tillförlitliga Service](service-fabric-reliable-services-introduction.md) och [tillförlitlig tjänst avancerade användning](service-fabric-reliable-services-advanced-usage.md) mer information om specifika för att skriva tjänster med hjälp av klasserna StatefulService och StatefulServiceBase.

Båda basklasser hantera livstid och rollen för tjänstimplementeringen. Tjänstimplementeringen kan åsidosätta virtuella metoder av antingen basklass om tjänstimplementeringen har att göra vid dessa punkter i tjänsten implementering livscykel-- eller om den vill skapa en lyssnare Kommunikationsobjekt. Att även om en implementering kan implementera sin egen Kommunikationsobjekt lyssnare exponera ICommunicationListener, i diagrammet ovan, lyssnaren kommunikation är implementeras av Service Fabric--som tjänsten implementering använder en kommunikation lyssnare som implementeras av Service Fabric.

En tillståndskänslig tillförlitlig tjänst använder hanteraren tillförlitliga tillstånd för att dra nytta av tillförlitliga samlingar. Tillförlitliga samlingar är lokala datastrukturer som hög tillgänglighet till tjänsten--som är, de är alltid tillgängliga, oavsett service växling vid fel. Varje typ av tillförlitliga samling implementeras av en tillförlitlig tillståndsprovidern.
Mer information om tillförlitlig samlingar finns i [tillförlitliga samlingar översikt](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Tillförlitliga tillståndshanterare och leverantörer av tillstånd
Hanteraren för tillförlitlig tillstånd är det objekt som hanterar tillförlitliga tillståndsprovidrar. Har funktioner för att skapa, ta bort, räkna upp och kontrollera att tillförlitliga tillståndsprovidrar är beständiga och hög tillgänglighet. En tillförlitlig tillstånd providerinstans representerar en instans av en beständig och hög tillgänglighet datastruktur, till exempel en ordlista eller en kö.

Varje tillförlitliga tillståndsprovidern exponerar ett gränssnitt som används av en tillståndskänslig service för att interagera med tillförlitlig tillståndsprovidern. Till exempel IReliableDictionary används gränssnittet med tillförlitlig ordlistan IReliableQueue används för gränssnittet med tillförlitlig kön. Alla tillförlitliga tillståndsprovidrar implementera gränssnittet IReliableState.

Den tillförlitliga tillståndshanterare har gränssnittet IReliableStateManager, vilket ger åtkomst till den från en tillståndskänslig service. Gränssnitt för att tillförlitligt tillståndsprovidrar returneras via IReliableStateManager.

Den tillförlitliga tillståndshanterare använder plugin arkitektur så att nya typer av tillförlitliga samlingar kan du ansluta dynamiskt.

Tillförlitliga ordlista och tillförlitlig kön bygger på implementering av en differentiell store hög prestanda, en ny version.

### <a name="transactional-replicator"></a>Transaktionell replikatorn
Den transaktionella replikatorkomponent ansvarar för att säkerställa att tillståndet för en tjänst (det vill säga tillstånd i hanteraren för tillförlitlig tillstånd och tillförlitlig samlingar) är konsekvent på alla repliker som kör tjänsten. Det innebär också att tillståndet sparas i loggen. Tillförlitliga tillstånd manager gränssnitt med transaktionella replikatorn via en mekanism för privat.

Transaktionell replikatorn använder nätverksprotokoll för att kommunicera tillstånd med andra repliker i tjänstinstansen så att alla repliker har aktuell statusinformation.

Transaktionell replikatorn använder en logg för att spara information om tillstånd så att tillståndet kvarstår process eller noden kraschar. Gränssnittet i loggen är via en mekanism för privat.

### <a name="log"></a>Logga
Loggkomponenten innehåller en högpresterande beständiga arkivet som kan optimeras för skrivning till snurrande eller SSD-diskar.  Design av loggen är för beständig lagring (d.v.s. hårddiskar) att de noder som kör tillståndskänslig service. Detta möjliggör låg latens och hög genomströmning jämfört med beständiga Fjärrlagring, som inte är lokal till noden.

Loggkomponenten använder flera loggfiler. Det finns en nod hela delade loggfil med alla repliker som den kan ge svarstid för lägsta och högsta dataflöde för att lagra användartillståndsdata. Som standard delade loggen placeras i katalogen Service Fabric-noden fungerar, men den kan också konfigureras så att den placeras på en annan plats, helst på en disk som är reserverade för delade loggen. Varje replik för tjänsten har också en dedikerad loggfil och dedikerad loggen placeras i tjänstens arbetskatalog. Det finns ingen mekanism för att konfigurera dedikerad loggen som ska placeras på en annan plats.

Delade loggen är ett övergående område för repliken statusinformation medan dedikerade loggfilen är slutdestinationen där du sparade. I den här designen tillståndsinformationen först skrivs till delade loggfilen och sedan lazy flyttades till dedikerade loggfilen i bakgrunden. På så sätt kan skulle skriva till delade loggen ha lägsta svarstid och högsta genomflödet som kan se förloppet snabbare-tjänsten.

Läser och skriver till delade loggen är klar via direkt i/o förallokerade utrymme på disken för delade loggfilen. Om du vill tillåta optimal användning av diskutrymme på enheten med dedikerade loggar skapas dedikerade loggfilen som en NTFS-sparse-fil. Observera att detta gör att överetablering diskutrymme och Operativsystemet visar de dedikerade loggfiler med mycket mer diskutrymme än vad som faktiskt används.

Utöver en minimal användarläge gränssnitt till loggen loggen skapas som en drivrutin för kernel-läge. Loggen kan ge högsta prestanda till alla tjänster som använder den genom att köra som en drivrutin för kernel-läge.

Mer information om hur du konfigurerar loggen finns [konfigurera tillståndskänsliga Reliable Services](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Tillståndslösa tillförlitlig tjänst
### <a name="architecture-of-a-stateless-service"></a>Arkitektur för tillståndslösa tjänsten
![Arkitekturdiagram för tillståndslösa tjänsten](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Tillståndslösa tillförlitlig tjänst
Implementeringar av tillståndslösa tjänsten härleds från klassen StatelessService eller StatelessServiceBase. Klassen StatelessServiceBase tillåter bättre flexibilitet än StatelessService-klassen.
Båda basklasser hantera livstid och rollen för en tjänst.

Tjänstimplementeringen kan åsidosätta virtuella metoder av antingen basklass om tjänsten har att göra vid dessa punkter i livscykeln för tjänsten-- eller om den vill skapa en lyssnare Kommunikationsobjekt. Observera att även om tjänsten kan implementera sin egen Kommunikationsobjekt lyssnare exponera ICommunicationListener, i diagrammet ovan, implementeras lyssnaren kommunikation av Service Fabric eftersom implementering av som använder en kommunikation lyssnare som implementeras av Service Fabric.

Finns det [översikt över tillförlitliga Service](service-fabric-reliable-services-introduction.md) och [tillförlitlig tjänst avancerade användning](service-fabric-reliable-services-advanced-usage.md) mer information om specifika för att skriva tjänster med hjälp av klasserna StatelessService och StatelessServiceBase.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nästa steg
Mer information om Service Fabric finns:

[Översikt över tillförlitliga service](service-fabric-reliable-services-introduction.md)

[Snabbstart](service-fabric-reliable-services-quick-start.md)

[Översikt över tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md)

[Tillförlitlig tjänst avancerad användning](service-fabric-reliable-services-advanced-usage.md)

[Tillförlitliga tjänstkonfiguration](service-fabric-reliable-services-configuration.md)  

