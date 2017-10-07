---
title: aaaReliable service-arkitektur | Microsoft Docs
description: "Översikt över hello tillförlitlig Service-arkitektur för tillståndskänsliga och tillståndslösa tjänster"
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
ms.openlocfilehash: d2d0ec9600275ae248ab7717be269cc7204a1e4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Arkitektur för tillståndskänsliga och tillståndslösa Reliable Services
En tillförlitlig tjänst för Azure Service Fabric kanske tillståndskänslig eller tillståndslösa. Varje typ av tjänst körs inom en viss arkitektur. Dessa arkitekturer beskrivs i den här artikeln.
Se hello [tillförlitliga översikt över](service-fabric-reliable-services-introduction.md) för mer information om hello skillnaderna mellan tillståndskänsliga och tillståndslösa tjänster.

## <a name="stateful-reliable-services"></a>Tillståndskänsliga Reliable Services
### <a name="architecture-of-a-stateful-service"></a>Arkitekturen för en tillståndskänslig service
![Arkitekturdiagram för en tillståndskänslig tjänst](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Tillståndskänsliga tillförlitlig tjänst
En tillståndskänslig tillförlitlig tjänst kan härledas från hello StatefulService eller StatefulServiceBase klass. Båda dessa basklasser som tillhandahålls av Service Fabric. De tillhandahåller olika nivåer av support och abstraktion för hello tillståndskänslig service toointerface med Service Fabric- och tooparticipate som en tjänst i hello Service Fabric-klustret.

StatefulService härleds från StatefulServiceBase. StatefulServiceBase ger bättre flexibilitet för tjänster, men kräver mer förståelse för hello internals av Service Fabric.
Se hello [översikt över tillförlitliga Service](service-fabric-reliable-services-introduction.md) och [tillförlitlig tjänst avancerade användning](service-fabric-reliable-services-advanced-usage.md) för mer hello detaljerad information för att skriva tjänster med hjälp av hello StatefulService och StatefulServiceBase klasser .

Båda basklasser hantera hello livstid och rollen för hello implementering. implementering av hello kan åsidosätta virtuella metoder av antingen basklass om implementering av hello har arbete toodo vid dessa punkter i livscykeln för implementering av hello tjänsten-- eller om den vill toocreate en lyssnare Kommunikationsobjekt. Observera att även om en implementering kan implementera sin egen Kommunikationsobjekt lyssnare exponera ICommunicationListener, i hello diagrammet ovan, hello kommunikation lyssnare implementeras av Service Fabric--som hello implementering använder en kommunikation lyssnare som implementeras av Service Fabric.

En tillståndskänslig tillförlitlig tjänst använder hello tillförlitliga tillstånd manager tootake nytta av tillförlitliga samlingar. Tillförlitliga samlingar är lokala datastrukturer som är hög tillgänglighet toohello tjänst--, de är alltid tillgängliga, oavsett service växling vid fel. Varje typ av tillförlitliga samling implementeras av en tillförlitlig tillståndsprovidern.
Mer information om tillförlitlig samlingar finns i hello [tillförlitliga samlingar översikt](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Tillförlitliga tillståndshanterare och leverantörer av tillstånd
hello tillförlitliga tillståndshanterare är hello-objekt som hanterar tillförlitliga tillståndsprovidrar. Den har hello funktioner toocreate, ta bort, räkna upp och se till att hello tillförlitliga tillståndsprovidrar är beständiga och hög tillgänglighet. En tillförlitlig tillstånd providerinstans representerar en instans av en beständig och hög tillgänglighet datastruktur, till exempel en ordlista eller en kö.

Varje tillförlitliga tillståndsprovidern exponerar ett gränssnitt som används av en tillståndskänslig service toointeract med hello tillförlitliga tillståndsprovidern. IReliableDictionary är till exempel använda toointerface tillförlitliga hello-ordlistan medan IReliableQueue används toointerface med tillförlitlig hello-kö. Alla tillförlitliga tillståndsprovidrar implementera hello IReliableState-gränssnittet.

hello tillförlitliga tillståndshanterare har gränssnittet IReliableStateManager som tillåter åtkomst tooit från en tillståndskänslig service. Gränssnitt tooreliable tillståndsprovidrar returneras via IReliableStateManager.

hello tillförlitliga tillståndshanterare använder plugin arkitektur så att nya typer av tillförlitliga samlingar kan du ansluta dynamiskt.

hello tillförlitliga ordlista och tillförlitlig kön bygger på hello implementering av en differentiell store hög prestanda, en ny version.

### <a name="transactional-replicator"></a>Transaktionell replikatorn
hello transaktionella replikatorkomponent ansvarar för att säkerställa att hello tillstånd för en tjänst (det vill säga hello tillstånd inom hello tillförlitliga tillståndshanterare och hello tillförlitliga samlingar) är konsekvent på alla repliker hello-tjänsten körs. Det innebär också att hello tillstånd sparas i hello-loggen. hello tillförlitliga tillstånd manager gränssnitt med hello transaktionella replikatorn via en mekanism för privat.

hello transaktionella replikatorn använder ett nätverk protokollet toocommunicate tillstånd med andra repliker av hello tjänstinstans så att alla repliker har aktuell statusinformation.

hello transaktionella replikatorn använder en logginformation toopersist tillstånd så att hello statusinformation FRISKRIVNINGEN process eller noden kraschar. hello gränssnittet toohello loggen är via en mekanism för privat.

### <a name="log"></a>Logga
hello loggkomponenten ger en hög prestanda beständiga arkivet som kan optimeras för att skriva toospinning eller SSD-diskar.  hello är hello loggen för hello beständig lagring (d.v.s. hårddiskar) toobe lokala toohello noder som kör hello tillståndskänslig service. Detta möjliggör låg latens och hög genomströmning som jämfört med tooremote beständig lagring, som inte är lokal toohello nod.

hello loggkomponenten använder flera loggfiler. Det finns en nod hela delade loggfil med alla repliker som den kan ge hello lägsta svarstid och högsta dataflöde för att lagra användartillståndsdata. Som standard hello delade loggen placeras i hello Service Fabric-noden fungerar directory men det kan också vara konfigurerade toobe placeras på en annan plats, helst på en disk som är reserverade för endast hello delade logg. Varje replik för hello-tjänsten har också en dedikerad loggfil och hello dedikerade loggen har placerats i arbetskatalogen hello-tjänsten. Det finns ingen mekanism tooconfigure hello dedikerad loggen toobe placeras på en annan plats.

hello delade loggen är ett övergående område för hello replik statusinformation, medan hello dedikerade loggfilen är hello slutdestinationen där du sparade. I den här designen hello statusinformation är första skriftliga toohello delade loggfilen och flyttas lazy toohello dedikerade loggfil i hello bakgrund. På så sätt kan skulle hello Skriv toohello delade loggfil ha hello lägsta svarstid och högsta dataflöde där hello service toomake förlopp snabbare.

Läser och skriver toohello delade loggen görs via direkt i/o toopreallocated diskutrymme hello hello delade loggfilen. tooallow optimal användning av diskutrymme på hello med dedikerade loggar hello dedikerade loggfil skapas som en NTFS-sparse-fil. Observera att detta gör att överetablering diskutrymme och hello OS visar hello dedikerad loggfiler med mycket mer diskutrymme än vad som faktiskt används.

Utöver en minimal användarläge gränssnittet toohello logg skapas hello loggen som en drivrutin för kernel-läge. Genom att köra en drivrutin för kernel-läge, hello loggen kan innehålla hello högsta prestanda tooall tjänster som använder den.

Mer information om hur du konfigurerar hello loggen finns [konfigurera tillståndskänsliga Reliable Services](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Tillståndslösa tillförlitlig tjänst
### <a name="architecture-of-a-stateless-service"></a>Arkitektur för tillståndslösa tjänsten
![Arkitekturdiagram för tillståndslösa tjänsten](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Tillståndslösa tillförlitlig tjänst
Implementeringar av tillståndslösa tjänsten härledas från hello StatelessService eller StatelessServiceBase klass. Hej StatelessServiceBase klassen tillåter bättre flexibilitet än hello StatelessService klass.
Båda basklasser hantera hello livstid och rollen för en tjänst.

implementering av hello kan åsidosätta virtuella metoder av antingen basklass om hello-tjänsten har arbete toodo vid dessa punkter i hello service lifecycle-- eller om den vill toocreate en lyssnare Kommunikationsobjekt. Observera att även om hello-tjänsten kan implementera sin egen Kommunikationsobjekt lyssnare exponera ICommunicationListener, i hello diagrammet ovan, implementeras hello kommunikation lyssnare av Service Fabric eftersom implementering av som använder en kommunikation lyssnare som implementeras av Service Fabric.

Se hello [översikt över tillförlitliga Service](service-fabric-reliable-services-introduction.md) och [tillförlitlig tjänst avancerade användning](service-fabric-reliable-services-advanced-usage.md) för mer hello detaljerad information för att skriva tjänster med hjälp av hello StatelessService och StatelessServiceBase klasser .

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
Mer information om Service Fabric finns:

[Översikt över tillförlitliga service](service-fabric-reliable-services-introduction.md)

[Snabbstart](service-fabric-reliable-services-quick-start.md)

[Översikt över tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md)

[Tillförlitlig tjänst avancerad användning](service-fabric-reliable-services-advanced-usage.md)

[Tillförlitliga tjänstkonfiguration](service-fabric-reliable-services-configuration.md)  

