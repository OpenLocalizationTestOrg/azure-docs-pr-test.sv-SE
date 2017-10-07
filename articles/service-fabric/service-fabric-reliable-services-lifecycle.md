---
title: "aaaOverview av hello livscykeln för Azure Service Fabric Reliable Services | Microsoft Docs"
description: "Lär dig mer om hello livscykeln för olika händelser i Service Fabric reliable services"
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek;
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 0d75ed5ee7cda85ac9af6a02e160727277804a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Översikt över livscykeln Reliable services
> [!div class="op_single_selector"]
> * [C# i Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java i Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

När du tänker hello livscykler för Reliable Services är hello grunderna i hello livscykel hello viktigaste. I allmänhet:

- Under Start
  - Tjänster är uppbyggda
  - De har en möjlighet tooconstruct och returnera noll eller flera lyssnare
  - Alla returnerade lyssnare öppnas kommunikation med hello-tjänsten
  - hello Service RunAsync metoden anropas, så att hello tjänsten toodo tidskrävande eller bakgrund arbete
- Vid avstängning
  - hello annullering token skickade tooRunAsync avbryts och hello-lyssnare har stängts
  - När detta är klar, hello service själva objektet är destructed

Det finns information kring hello exakt sorteringen av dessa händelser. I synnerhet kan hello ordningen för händelser ändras lite beroende på om hello tillförlitlig tjänst är Stateless eller Stateful. Dessutom för tillståndskänsliga tjänster har vi toodeal med hello primära växlingen scenario. Under den här sekvensen hello rollen som primär är överförda tooanother replik (eller kommer tillbaka) utan hello-tjänsten avslutas. Slutligen har vi toothink om felet villkor.

## <a name="stateless-service-startup"></a>Tillståndslösa tjänsten startades
hello livscykeln för tillståndslösa tjänsten är ganska enkelt. Här är hello efter händelser:

1. hello har tjänsten skapats
2. Görs sedan i parallella två saker:
    - `StatelessService.CreateServiceInstanceListeners()`har anropats och eventuella returnerade lyssnare öppnas. `ICommunicationListener.OpenAsync()`anropas på varje lyssnare
    - Hej tjänstens `StatelessService.RunAsync()` metoden anropas
3. Om den finns hello tjänstens `StatelessService.OnOpenAsync()` metoden anropas. Det här är en ovanligt åsidosättning, men den är tillgänglig.

Det är viktigt toonote att det finns ingen ordning mellan hello anrop toocreate och öppna hello-lyssnare och RunAsync. hello lyssnare kan öppna innan RunAsync har startats. På liknande sätt kan RunAsync hamna anropas innan hello kommunikationslyssnarna är öppna eller även har skapats. Om ingen synkronisering krävs lämnas den som en övning toohello implementerare. Vanliga lösningar:

  - Lyssnare fungerar ibland inte förrän annan information som har skapats eller arbete som gjorts. För tillståndslösa tjänster som arbete kan vanligtvis utföras på andra platser som: 
    - i hello service-konstruktorn
    - under hello `CreateServiceInstanceListeners()` anropa
    - som en del av hello konstruktionen av hello listener sig själv
  - Ibland vill hello koden i RunAsync inte toostart tills hello lyssnare är öppna. I så fall krävs ytterligare samordning. En vanlig lösning är vissa flagga i hello lyssnare som anger när de har slutförts. Den här flaggan kontrolleras sedan i RunAsync innan du fortsätter tooactual arbete.

## <a name="stateless-service-shutdown"></a>Tillståndslösa tjänsten avstängning
När du stänger en tillståndslös tjänst hello samma mönster följs bara i omvänd:

1. Parallellt
    - Alla öppna lyssnare stängs. `ICommunicationListener.CloseAsync()`anropas på varje lyssnare.
    - hello annullering token som skickades för`RunAsync()` har avbrutits. Kontrollerar hello annullering token `IsCancellationRequested` -egenskap returnerar true, och om den anropas hello token `ThrowIfCancellationRequested` metoden returnerar en `OperationCanceledException`.
2. En gång `CloseAsync()` har slutförts på varje lyssnare och `RunAsync()` också har slutförts hello service `StatelessService.OnCloseAsync()` metoden anropas, om sådan finns. Det är ovanligt toooverride `StatelessService.OnCloseAsync()`.
3. När `StatelessService.OnCloseAsync()` är klar hello webbtjänstobjektet destructed

## <a name="stateful-service-startup"></a>Tillståndskänslig service Start
Tillståndskänsliga tjänster har en liknande mönster toostateless tjänster, med några ändringar. När du startar en tillståndskänslig service är hello händelser följande:

1. hello har tjänsten skapats
2. `StatefulServiceBase.OnOpenAsync()`anropas. Detta åsidosätts uncommonly i hello-tjänsten.
3. hello händer följande parallellt
    - `StatefulServiceBase.CreateServiceReplicaListeners()`anropas 
      - Om hello-tjänsten är en primär öppnas alla returnerade lyssnare. `ICommunicationListener.OpenAsync()`anropas på varje lyssnare.
      - Om hello-tjänsten är en sekundär kan endast dessa lyssnare markerad som `ListenOnSecondary = true` öppnas. Med lyssnare som är öppna på sekundärservrar är mindre vanliga.
    - Hej om hello Service är en primär, hello tjänst `StatefulServiceBase.RunAsync()` metoden anropas
4. När alla hello replik lyssnare `OpenAsync()` anrop slutföra och `RunAsync()` anropas, `StatefulServiceBase.OnChangeRoleAsync()` anropas. Detta åsidosätts uncommonly i hello-tjänsten.

På liknande sätt toostateless tjänster, det finns inga samordning mellan hello ordning i vilken hello lyssnare skapas och öppnas och när RunAsync anropas. Om du behöver samordning, är det mycket hello samma hello lösningar. Det finns en ytterligare fall: säger att hello anrop anländer till hello kommunikationslyssnarna kräver information som sparas i vissa [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md). Vissa ytterligare samordning är nödvändigt eftersom hello kommunikationslyssnarna gick att öppna innan hello tillförlitliga samlingar är läsbart eller skrivbart och innan RunAsync kunde starta. hello enklaste och de vanligaste lösningen är för hello kommunikation lyssnare tooreturn vissa felkod som hello använder tooknow tooretry hello klientbegäran.

## <a name="stateful-service-shutdown"></a>Tillståndskänslig service avstängning
På samma sätt tooStateless tjänster hello Livscykelhändelser vid avstängningen är hello samma som under starten, men omvänd. När en tillståndskänslig service stängs av, händer hello följande:

1. Parallellt
    - Alla öppna lyssnare stängs. `ICommunicationListener.CloseAsync()`anropas på varje lyssnare.
    - hello annullering token som skickades för`RunAsync()` har avbrutits. Kontrollerar hello annullering token `IsCancellationRequested` -egenskap returnerar true, och om den anropas hello token `ThrowIfCancellationRequested` metoden returnerar en `OperationCanceledException`.
2. En gång `CloseAsync()` har slutförts på varje lyssnare och `RunAsync()` också har slutförts hello service `StatefulServiceBase.OnChangeRoleAsync()` anropas. (Detta uncommonly åsidosätts i hello service.)
    - Väntar på att RunAsync är toocomplete bara nödvändigt om den här tjänsten repliken var en primär.
3. En gång hello `StatefulServiceBase.OnChangeRoleAsync()` metoden har slutförts hello `StatefulServiceBase.OnCloseAsync()` metoden anropas. Det här är en ovanligt åsidosättning, men den är tillgänglig.
3. När `StatefulServiceBase.OnCloseAsync()` är klar hello webbtjänstobjektet destructed.

## <a name="stateful-service-primary-swaps"></a>Tillståndskänslig service primära växlingar
När en tillståndskänslig tjänst körs bara ha hello primära repliker av de tillståndskänsliga tjänsterna sina kommunikationslyssnarna öppnas och deras RunAsync-metoden anropas. Sekundär är uppbyggda men det finns inga ytterligare anrop. När en tillståndskänslig tjänst körs men, kan vilka repliken är för närvarande hello primära ändra. Vad innebär det i termer av hello Livscykelhändelser som en replik kan se? hello beteende hello tillståndskänslig replik ser beror på om det är hello replik som nedgraderas eller uppgraderas under hello växlingen.

### <a name="for-hello-primary-being-demoted"></a>För hello primära som nedgraderas
Service Fabric måste den här repliken toostop meddelandebehandling och avsluta alla bakgrundsjobbet avbrytas. Det här steget ser ut liknande toowhen hello-tjänsten stängs av. En skillnaden är att hello Service inte är destructed eller stängts eftersom det fortfarande som en sekundär. hello följande API: er anropas:

1. Parallellt
    - Alla öppna lyssnare stängs. `ICommunicationListener.CloseAsync()`anropas på varje lyssnare.
    - hello annullering token som skickades för`RunAsync()` har avbrutits. Kontrollerar hello annullering token `IsCancellationRequested` -egenskap returnerar true, och om den anropas hello token `ThrowIfCancellationRequested` metoden returnerar en `OperationCanceledException`.
2. En gång `CloseAsync()` har slutförts på varje lyssnare och `RunAsync()` också har slutförts hello service `StatefulServiceBase.OnChangeRoleAsync()` anropas. Detta åsidosätts uncommonly i hello-tjänsten.

### <a name="for-hello-secondary-being-promoted"></a>För sekundära hello som uppgraderas
På liknande sätt, Service Fabric måste den här repliken toostart lyssnar efter meddelanden på hello överföring och starta alla bakgrundsaktiviteter den mån om. Därför kan den här processen ser ut som liknande toowhen hello tjänst skapas, utom hello repliken finns själva redan. hello följande API: er anropas:

1. Parallellt
    - `StatefulServiceBase.CreateServiceReplicaListeners()`har anropats och eventuella returnerade lyssnare öppnas. `ICommunicationListener.OpenAsync()`anropas på varje lyssnare.
    - Hej tjänstens `StatefulServiceBase.RunAsync()` metoden anropas
2. När alla hello replik lyssnare `OpenAsync()` anrop slutföra och `RunAsync()` har anropats `StatefulServiceBase.OnChangeRoleAsync()` anropas. Detta åsidosätts uncommonly i hello-tjänsten.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Vanliga problem vid avstängning av tjänst tillståndskänsliga och primära degradering
Service Fabric ändringar hello primära av en tillståndskänslig tjänst för en mängd olika skäl. hello vanligaste är [kluster ombalansering](service-fabric-cluster-resource-manager-balancing.md) och [Programuppgradering](service-fabric-application-upgrade.md). Under dessa åtgärder (samt under normal drift avslutning, som du vill se om hello-tjänsten har tagits bort), är det viktigt att hello service avseende hello `CancellationToken`. Tjänster som inte hanterar avbryta en klar får flera problem. I synnerhet dessa åtgärder kommer att vara långsam eftersom Service Fabric väntar hello services toostop avslutas. Slutligen kan detta leda toofailed uppgraderingar tiden och återställa. Fel toohonor hello annullering token kan också medföra imbalanced kluster eftersom noder hämta hot men kan inte vara genomförs hello services eftersom det tar för lång toomove dem någon annanstans. 

Eftersom hello tjänster är tillståndskänslig, det är också troligt att de använder hello [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md). När en primär degraderas i Service Fabric är hello första saker händer att tillståndet för skrivåtkomst toohello underliggande har återkallats. Detta leder tooa andra uppsättning problem som kan påverka hello service lifecycle. hello samlingar returnerade undantag baserat på hello tidsinställning och om hello replik flyttas eller stängs av. Sådana undantag ska hanteras på rätt sätt. Undantag från Service Fabric är indelade i permanent [(`FabricException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) och tillfälligt [(`FabricTransientException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) kategorier. Permanent undantag ska loggas och uppstod medan hello tillfälligt undantag kan göras baserat på vissa logik.

Hantera hello undantag som härrör från användning av hello `ReliableCollections` tillsammans med tjänsten Livscykelhändelser är en viktig del av testa och validera en tillförlitlig tjänst. Hej rekommendation är alltid toorun din tjänst under belastning under uppgraderingar och [chaos testning](service-fabric-controlled-chaos.md) innan du distribuerar någonsin tooproduction. De grundläggande stegen för att säkerställa att din tjänst har implementerats korrekt och hanterar Livscykelhändelser på rätt sätt.


## <a name="notes-on-service-lifecycle"></a>Information om tjänsten livscykel
  - Båda hello `RunAsync()` metod och hello `CreateServiceReplicaListeners/CreateServiceInstanceListeners` anrop är valfria. En tjänst kan ha ett av dem, båda eller inget. Till exempel om hello tjänsten gör allt arbete i svaret toouser anrop, det behövs ingen för den tooimplement `RunAsync()`. Endast hello kommunikationslyssnarna och deras associerade kod krävs. På liknande sätt att skapa och returnera kommunikationslyssnarna är valfritt, eftersom hello service kanske bara bakgrunden fungerar toodo och så att endast måste tooimplement`RunAsync()`
  - Det är giltigt för en tjänst toocomplete `RunAsync()` har och RETUR från den. Slutför är inte ett fel inträffar. Slutför `RunAsync()` visar att hello bakgrundsjobbet av hello-tjänsten har slutförts. För tillståndskänsliga reliable services `RunAsync()` anropas igen om hello replik degraderades från primära tooSecondary och upphöja tillbaka tooPrimary.
  - Om en tjänst avslutas från `RunAsync()` genom att vissa oväntat undantag utgör detta ett fel. webbtjänstobjektet hello stängs av och rapporterade felet hälsa.
  - När det finns ingen tidsgräns på returneras från dessa metoder kan du att förlora omedelbart hello möjlighet toowrite tooReliable samlingar och därför slutföra inte något verkligt arbete. Vi rekommenderar att du returnera så snabbt som möjligt när hello annullering begäran tas emot. Om tjänsten inte svarar toothese API-anrop inom en rimlig tid Service Fabric kan framtvinga stänga av tjänsten. Vanligtvis sker detta endast under programuppgraderingar eller när en tjänst tas bort. Det här är 15 minuter som standard.
  - Fel i hello `OnCloseAsync()` sökväg resultatet i `OnAbort()` anropas som är ett sista chansen bästa möjlighet för hello tjänsten tooclean upp och frigör alla resurser som de har begärts.

## <a name="next-steps"></a>Nästa steg
- [Introduktion tooReliable tjänster](service-fabric-reliable-services-introduction.md)
- [Snabbstart för Reliable Services](service-fabric-reliable-services-quick-start.md)
- [Reliable Services avancerade användning](service-fabric-reliable-services-advanced-usage.md)
