---
title: "aaaOverview av hello livscykeln för Azure Service Fabric Reliable Services | Microsoft Docs"
description: "Lär dig mer om hello livscykeln för olika händelser i Service Fabric reliable services"
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: 6d48c217d12bc5248c2da57b544aac747cecd872
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

* Under Start
  * Tjänster är uppbyggda
  * De har en möjlighet tooconstruct och returnera noll eller flera lyssnare
  * Alla returnerade lyssnare öppnas kommunikation med hello-tjänsten
  * hello Service runAsync-metoden anropas, så att hello service toodo tidskrävande eller bakgrund arbete
* Vid avstängning
  * hello annullering token skickade toorunAsync avbryts och hello-lyssnare har stängts
  * När detta är klar, hello service själva objektet är destructed

Det finns information kring hello exakt sorteringen av dessa händelser. I synnerhet kan hello ordningen för händelser ändras lite beroende på om hello tillförlitlig tjänst är Stateless eller Stateful. Dessutom för tillståndskänsliga tjänster har vi toodeal med hello primära växlingen scenario. Under den här sekvensen hello rollen som primär är överförda tooanother replik (eller kommer tillbaka) utan hello-tjänsten avslutas. Slutligen har vi toothink om felet villkor.

## <a name="stateless-service-startup"></a>Tillståndslösa tjänsten startades
hello livscykeln för tillståndslösa tjänsten är ganska enkelt. Här är hello efter händelser:

1. hello har tjänsten skapats
2. Görs sedan i parallella två saker:
    - `StatelessService.createServiceInstanceListeners()`har anropats och eventuella returnerade lyssnare öppnas (`CommunicationListener.openAsync()` anropas på varje lyssnare)
    - Hej tjänstens runAsync metod (`StatelessService.runAsync()`) anropas
3. Om den finns hello tjänstens egna onOpenAsync metoden anropas (särskilt `StatelessService.onOpenAsync()` anropas. Det här är en ovanligt åsidosättning men det är tillgängligt).

Det är viktigt toonote att det finns ingen ordning mellan hello anrop toocreate och öppna hello-lyssnare och runAsync. hello lyssnare kan öppna innan runAsync har startats. På liknande sätt kan runAsync hamna anropas innan hello kommunikationslyssnarna är öppna eller även har skapats. Om ingen synkronisering krävs lämnas den som en övning toohello implementerare. Vanliga lösningar:

* Lyssnare fungerar ibland inte förrän annan information som har skapats eller arbete som gjorts. För tillståndslösa tjänster som arbete kan vanligtvis utföras i hello service konstruktorn under hello `createServiceInstanceListeners()` anropar, eller som en del av hello konstruktionen av hello listener sig själv.
* Ibland vill hello koden i runAsync inte toostart tills hello lyssnare är öppna. I så fall krävs ytterligare samordning. En vanlig lösning är vissa flagga i hello lyssnare som anger när de har slutfört, som är markerad i runAsync innan du fortsätter tooactual arbete.

## <a name="stateless-service-shutdown"></a>Tillståndslösa tjänsten avstängning
När du stänger en tillståndslös tjänst hello samma mönster följs bara i omvänd:

1. Parallellt
    - Stängs alla öppna lyssnare (`CommunicationListener.closeAsync()` anropas på varje lyssnare)
    - hello annullering token som skickades för`runAsync()` avbryts (kontrollerar hello annullering token `isCancelled` -egenskap returnerar true, och om den anropas hello token `throwIfCancellationRequested` metoden returnerar en `CancellationException`)
2. En gång `closeAsync()` har slutförts på varje lyssnare och `runAsync()` också har slutförts hello service `StatelessService.onCloseAsync()` metoden anropas, om det finns (igen det här är en ovanligt åsidosättning).
3. När `StatelessService.onCloseAsync()` är klar hello webbtjänstobjektet destructed

## <a name="notes-on-service-lifecycle"></a>Information om tjänsten livscykel
* Båda hello `runAsync()` metod och hello `createServiceInstanceListeners` anrop är valfria. En tjänst kan ha ett av dem, båda eller inget. Till exempel om hello tjänsten gör allt arbete i svaret toouser anrop, det behövs ingen för den tooimplement `runAsync()`. Endast hello kommunikationslyssnarna och deras associerade kod krävs. På liknande sätt att skapa och returnera kommunikationslyssnarna är valfritt, eftersom hello service kanske bara bakgrunden fungerar toodo och så att endast måste tooimplement`runAsync()`
* Det är giltigt för en tjänst toocomplete `runAsync()` har och RETUR från den. Detta anses inte vara ett fel inträffar och representerar hello bakgrundsjobbet hello service slutföra. För tillståndskänsliga reliable services `runAsync()` skulle anropas igen om hello tjänsten degraderades från primära och upphöja tillbaka tooprimary.
* Om en tjänst avslutas från `runAsync()` genom att vissa oväntade undantag, detta är ett fel och hello serviceobjektet är avstängd och ett hälsotillstånd fel rapporteras.
* När det finns ingen tidsgräns på returneras från dessa metoder kan du att förlora omedelbart hello möjlighet toowrite och därför slutföra inte något verkligt arbete. Vi rekommenderar att du returnera så snabbt som möjligt när hello annullering begäran tas emot. Om tjänsten inte svarar toothese API-anrop inom en rimlig tid Service Fabric kan framtvinga stänga av tjänsten. Vanligtvis sker detta endast under programuppgraderingar eller när en tjänst tas bort. Det här är 15 minuter som standard.
* Fel i hello `onCloseAsync()` sökväg resultatet i `onAbort()` anropas som är ett sista chansen bästa möjlighet för hello tjänsten tooclean upp och frigör alla resurser som de har begärts.

> [!NOTE]
> Tillståndskänsliga reliable services stöds inte i java ännu.
>
>

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooReliable tjänster](service-fabric-reliable-services-introduction.md)
* [Snabbstart för Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Reliable Services avancerade användning](service-fabric-reliable-services-advanced-usage.md)
