---
title: "Översikt över livscykeln för Azure Service Fabric Reliable Services | Microsoft Docs"
description: "Lär dig mer om livscykeln för olika händelser i Service Fabric reliable services"
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
ms.openlocfilehash: 16021ca72a2f10070b6409417ff0d88009591331
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Översikt över livscykeln Reliable services
> [!div class="op_single_selector"]
> * [C# i Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java i Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Grunderna i livscykeln är viktigast när du tänker livscykler för Reliable Services. I allmänhet:

- Under Start
  - Tjänster är uppbyggda
  - De har möjlighet att skapa och returnera noll eller flera lyssnare
  - Alla returnerade lyssnare öppnas kommunikation med tjänsten
  - Tjänstens RunAsync-metoden anropas, vilket gör att tjänsten för att göra länge körs eller bakgrund arbete
- Vid avstängning
  - Annullering-token som skickades till RunAsync avbryts och lyssnarna stängs
  - När detta är klar, själva tjänstobjektet är destructed

Det finns information runt den exakta sorteringen av dessa händelser. I synnerhet kan ordningen för händelser ändras lite beroende på om tjänsten tillförlitliga är Stateless eller Stateful. Dessutom för tillståndskänsliga tjänster måste vi hantera primära växlingen scenario. Under den här sekvensen rollen som primär överförs till en annan replik (eller kommer tillbaka) utan att tjänsten avslutas. Slutligen behöver vi tänka på felet villkor.

## <a name="stateless-service-startup"></a>Tillståndslösa tjänsten startades
Livscykeln för tillståndslösa tjänsten är ganska enkelt. Här är ordningen för händelser:

1. Tjänsten har skapats
2. Görs sedan i parallella två saker:
    - `StatelessService.CreateServiceInstanceListeners()`har anropats och eventuella returnerade lyssnare öppnas. `ICommunicationListener.OpenAsync()`anropas på varje lyssnare
    - Tjänstens `StatelessService.RunAsync()` metoden anropas
3. Om den finns i tjänstens `StatelessService.OnOpenAsync()` metoden anropas. Det här är en ovanligt åsidosättning, men den är tillgänglig.

Det är viktigt att notera att det finns ingen ordning mellan anrop för att skapa och öppna lyssnare och RunAsync. Lyssnarna kan öppna innan RunAsync har startats. På liknande sätt kan RunAsync hamna anropas innan kommunikationslyssnarna är öppna eller även har skapats. Om ingen synkronisering krävs, måste lämnas den som Övning till Implementeraren. Vanliga lösningar:

  - Lyssnare fungerar ibland inte förrän annan information som har skapats eller arbete som gjorts. För tillståndslösa tjänster som arbete kan vanligtvis utföras på andra platser som: 
    - i tjänstens konstruktor
    - under den `CreateServiceInstanceListeners()` anropa
    - som en del av konstruktionen för lyssnare sig själv
  - Ibland vill koden i RunAsync inte startas förrän lyssnarna är öppna. I så fall krävs ytterligare samordning. En vanlig lösning är vissa flagga i lyssnare som anger när de har slutförts. Den här flaggan kontrolleras sedan i RunAsync innan du fortsätter verkligt arbete.

## <a name="stateless-service-shutdown"></a>Tillståndslösa tjänsten avstängning
När du stänger en tillståndslös tjänst följt samma mönster bara i omvänd:

1. Parallellt
    - Alla öppna lyssnare stängs. `ICommunicationListener.CloseAsync()`anropas på varje lyssnare.
    - Annullering-token som skickades till `RunAsync()` har avbrutits. Kontrollera token annullering `IsCancellationRequested` -egenskap returnerar true, och om den anropas token `ThrowIfCancellationRequested` metoden returnerar en `OperationCanceledException`.
2. En gång `CloseAsync()` har slutförts på varje lyssnare och `RunAsync()` också har slutförts tjänstens `StatelessService.OnCloseAsync()` metoden anropas, om sådan finns. Det är ovanligt att åsidosätta `StatelessService.OnCloseAsync()`.
3. När `StatelessService.OnCloseAsync()` är klar serviceobjektet destructed

## <a name="stateful-service-startup"></a>Tillståndskänslig service Start
Tillståndskänsliga tjänster har ett liknande mönster tillståndslösa tjänster med några ändringar. När du startar en tillståndskänslig service är ordningen för händelser:

1. Tjänsten har skapats
2. `StatefulServiceBase.OnOpenAsync()`anropas. Detta åsidosätts uncommonly i tjänsten.
3. Följande händer parallellt
    - `StatefulServiceBase.CreateServiceReplicaListeners()`anropas 
      - Om tjänsten är en primär öppnas alla returnerade lyssnare. `ICommunicationListener.OpenAsync()`anropas på varje lyssnare.
      - Om tjänsten är en sekundär kan endast dessa lyssnare markerad som `ListenOnSecondary = true` öppnas. Med lyssnare som är öppna på sekundärservrar är mindre vanliga.
    - Den om tjänsten är en primär, tjänstens `StatefulServiceBase.RunAsync()` metoden anropas
4. När alla replik lyssnarens `OpenAsync()` anrop slutföra och `RunAsync()` anropas, `StatefulServiceBase.OnChangeRoleAsync()` anropas. Detta åsidosätts uncommonly i tjänsten.

På liknande sätt till tillståndslösa tjänster finns inga samordning mellan ordningen som lyssnarna har skapats och öppnats och när RunAsync anropas. Om du behöver samordning är ungefär lösningarna. Det finns en ytterligare fall: säger att anrop anländer till kommunikationslyssnarna kräver information som sparas i vissa [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md). Vissa ytterligare samordning är nödvändigt eftersom kommunikationslyssnarna gick att öppna innan de tillförlitliga samlingarna är läsbart eller skrivbart och innan RunAsync kunde starta. Den enklaste och de vanligaste lösningen är för kommunikationslyssnarna att returnera vissa felkod som klienten använder för att försöka.

## <a name="stateful-service-shutdown"></a>Tillståndskänslig service avstängning
På samma sätt som tillståndslösa tjänster Livscykelhändelser vid avstängningen är samma som under starten men omvänd. När en tillståndskänslig service stängs av, händer följande:

1. Parallellt
    - Alla öppna lyssnare stängs. `ICommunicationListener.CloseAsync()`anropas på varje lyssnare.
    - Annullering-token som skickades till `RunAsync()` har avbrutits. Kontrollera token annullering `IsCancellationRequested` -egenskap returnerar true, och om den anropas token `ThrowIfCancellationRequested` metoden returnerar en `OperationCanceledException`.
2. En gång `CloseAsync()` har slutförts på varje lyssnare och `RunAsync()` också har slutförts tjänstens `StatefulServiceBase.OnChangeRoleAsync()` anropas. (Detta uncommonly åsidosätts i tjänsten.)
    - Väntar på RunAsync att slutföra är bara nödvändigt om den här tjänsten repliken var en primär.
3. En gång i `StatefulServiceBase.OnChangeRoleAsync()` metoden har slutförts i `StatefulServiceBase.OnCloseAsync()` -metoden anropas. Det här är en ovanligt åsidosättning, men den är tillgänglig.
3. När `StatefulServiceBase.OnCloseAsync()` är klar serviceobjektet destructed.

## <a name="stateful-service-primary-swaps"></a>Tillståndskänslig service primära växlingar
När en tillståndskänslig service körs, har de primära replikerna av de tillståndskänsliga tjänsterna sina kommunikationslyssnarna öppnas och deras RunAsync-metoden anropas. Sekundär är uppbyggda men det finns inga ytterligare anrop. När en tillståndskänslig tjänst körs men, kan vilka repliken för närvarande är primärt ändra. Vad innebär det i villkoren för händelserna livscykel som en replik kan se? Beteendet tillståndskänslig repliken ser beror på om det är repliken som nedgraderas eller uppgraderas under växlingen.

### <a name="for-the-primary-being-demoted"></a>För den primära som nedgraderas
Service Fabric måste den här repliken för att avbryta behandlar meddelanden och avsluta alla bakgrundsjobbet avbrytas. Därför kan liknar det här steget när tjänsten stängs av. En skillnaden är att tjänsten inte är destructed eller stängts eftersom det fortfarande som en sekundär. Följande API: er anropas:

1. Parallellt
    - Alla öppna lyssnare stängs. `ICommunicationListener.CloseAsync()`anropas på varje lyssnare.
    - Annullering-token som skickades till `RunAsync()` har avbrutits. Kontrollera token annullering `IsCancellationRequested` -egenskap returnerar true, och om den anropas token `ThrowIfCancellationRequested` metoden returnerar en `OperationCanceledException`.
2. En gång `CloseAsync()` har slutförts på varje lyssnare och `RunAsync()` också har slutförts tjänstens `StatefulServiceBase.OnChangeRoleAsync()` anropas. Detta åsidosätts uncommonly i tjänsten.

### <a name="for-the-secondary-being-promoted"></a>För den sekundära som uppgraderas
På liknande sätt, Service Fabric måste replikeringen att starta lyssnar efter meddelanden på kabeln och starta alla bakgrundsaktiviteter den mån om. Därför kan liknar den här processen när tjänsten har skapats, förutom att repliken själva finns redan. Följande API: er anropas:

1. Parallellt
    - `StatefulServiceBase.CreateServiceReplicaListeners()`har anropats och eventuella returnerade lyssnare öppnas. `ICommunicationListener.OpenAsync()`anropas på varje lyssnare.
    - Tjänstens `StatefulServiceBase.RunAsync()` metoden anropas
2. När alla replik lyssnarens `OpenAsync()` anrop slutföra och `RunAsync()` har anropats `StatefulServiceBase.OnChangeRoleAsync()` anropas. Detta åsidosätts uncommonly i tjänsten.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Vanliga problem vid avstängning av tjänst tillståndskänsliga och primära degradering
Service Fabric ändringar av en tillståndskänslig tjänst för en mängd olika skäl. De vanligaste är [kluster ombalansering](service-fabric-cluster-resource-manager-balancing.md) och [Programuppgradering](service-fabric-application-upgrade.md). Under dessa åtgärder (samt under normal drift avslutning, som du vill se om tjänsten har tagits bort), är det viktigt att tjänsten följer den `CancellationToken`. Tjänster som inte hanterar avbryta en klar får flera problem. I synnerhet dessa åtgärder kommer att vara långsam eftersom Service Fabric väntar tjänster ska avslutas. Slutligen kan detta leda till misslyckade uppgraderingar tiden och återställa. Respektera cancellation-token kan också medföra imbalanced kluster eftersom noder hämta hot men kan inte vara genomförs tjänsterna eftersom det tar för lång tid att flytta dem någon annanstans. 

Eftersom tjänsterna är tillståndskänslig, det är också troligt att de använder den [tillförlitliga samlingar](service-fabric-reliable-services-reliable-collections.md). När en primär degraderas i Service Fabric är en av de första sakerna som händer skrivåtkomst till tillståndet för underliggande återkallas. Detta leder till en annan uppsättning problem som kan påverka service lifecycle. Samlingar returnerade undantag baserat på tiden och om repliken flyttas eller stängs av. Sådana undantag ska hanteras på rätt sätt. Undantag från Service Fabric är indelade i permanent [(`FabricException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) och tillfälligt [(`FabricTransientException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) kategorier. Permanent undantag ska loggas och uppstod medan tillfälligt undantag kan göras baserat på vissa logik.

Hantering av undantag som härrör från användning av den `ReliableCollections` tillsammans med tjänsten Livscykelhändelser är en viktig del av testa och validera en tillförlitlig tjänst. Rekommendationen är alltid att köra tjänsten under belastning under uppgraderingar och [chaos testning](service-fabric-controlled-chaos.md) innan du distribuerar någonsin till produktionen. De grundläggande stegen för att säkerställa att din tjänst har implementerats korrekt och hanterar Livscykelhändelser på rätt sätt.


## <a name="notes-on-service-lifecycle"></a>Information om tjänsten livscykel
  - Både den `RunAsync()` metod och `CreateServiceReplicaListeners/CreateServiceInstanceListeners` anrop är valfria. En tjänst kan ha ett av dem, båda eller inget. Till exempel om tjänsten gör allt arbete som svar på användaren anropar det behövs ingen att implementera `RunAsync()`. Endast kommunikationslyssnarna och deras associerade kod krävs. På liknande sätt är att skapa och returnera kommunikationslyssnarna valfritt, eftersom tjänsten kan ha endast bakgrundsjobbet att göra och så att endast måste implementera`RunAsync()`
  - Det är giltigt för en tjänst att slutföra `RunAsync()` har och RETUR från den. Slutför är inte ett fel inträffar. Slutför `RunAsync()` visar att bakgrundsjobbet för tjänsten har slutförts. För tillståndskänsliga reliable services `RunAsync()` anropas igen om repliken har nedgraderats från primär till sekundär och sedan befordras till primära.
  - Om en tjänst avslutas från `RunAsync()` genom att vissa oväntat undantag utgör detta ett fel. Serviceobjektet stängs av och rapporterade felet hälsa.
  - När det finns ingen tidsgräns på returneras från dessa metoder kan du att förlora omedelbart möjligheten att skriva till tillförlitliga samlingar och därför slutföra inte något verkligt arbete. Vi rekommenderar att du returnera så snabbt som möjligt vid mottagning av begäran om att avbryta. Om tjänsten inte svarar på dessa API-anrop i rimlig tid Service Fabric tvång att avbryta tjänsten. Vanligtvis sker detta endast under programuppgraderingar eller när en tjänst tas bort. Det här är 15 minuter som standard.
  - Fel i den `OnCloseAsync()` sökväg resultatet i `OnAbort()` anropas som är ett sista chansen bästa möjligheten för tjänsten för att rensa upp och frigör alla resurser som de har begärts.

## <a name="next-steps"></a>Nästa steg
- [Introduktion till Reliable Services](service-fabric-reliable-services-introduction.md)
- [Snabbstart för Reliable Services](service-fabric-reliable-services-quick-start.md)
- [Reliable Services avancerade användning](service-fabric-reliable-services-advanced-usage.md)
