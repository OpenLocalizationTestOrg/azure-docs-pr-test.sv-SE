---
title: "Översikt över aaaFault Analysis Service | Microsoft Docs"
description: "Den här artikeln beskriver hello fel Analysis Service i Service Fabric för att fel och kör testscenarier mot dina tjänster."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: timlt
editor: vturecek
ms.assetid: 1f064276-293a-4989-a513-e0d0b9fdf703
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: deac16ec830aa10d4e488e60691faa9ef2b6cd33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-fault-analysis-service"></a>Introduktion toohello fel Analysis Service
hello fel Analysis Service är utformad för att testa tjänster som bygger på Microsoft Azure Service Fabric. Du kan använda hello fel Analysis Service för att framkalla meningsfulla fel och köra scenarier för testning mot dina program. Dessa fel och scenarier utöva och validera hello många tillstånd och övergångar som en tjänst får under dess livslängd i en kontrollerad, säker och konsekvent sätt.

Åtgärder är hello enskilda fel inriktning på en tjänst för att testa den. En service-utvecklare kan använda dessa som byggblock toowrite komplicerade scenarier. Exempel:

* Starta om en nod toosimulate valfritt antal situationer där en dator eller virtuell dator startas.
* Flytta en replik av din tillståndskänslig service toosimulate belastningsutjämning, redundans eller uppgradering av programmet.
* Anropa förlorar kvorum på tillståndskänslig service-toocreate en situation där skrivåtgärder inte kan fortsätta eftersom det inte finns tillräckligt med ”säkerhetskopiering” eller ”sekundär” repliker tooaccept nya data.
* Anropa förlust av data på en tillståndskänslig service toocreate en situation där alla InMemory-tillstånd rensas helt ut.

Scenarier är komplexa åtgärder som består av en eller flera åtgärder. hello fel Analysis Services innehåller två inbyggda kompletta scenarier:

* Chaos Scenario
* Scenario för växling vid fel

## <a name="testing-as-a-service"></a>Testa som en tjänst
hello fel Analysis Service är en tjänst för Service Fabric system som startas automatiskt med ett Service Fabric-kluster. Den här tjänsten fungerar som hello värden för fel injection och scenario testkörning hälsoanalys. 

![Fel Analysis Service][0]

När ett fel åtgärd eller test-scenario initieras, skickas ett kommando toohello fel Analysis Service toorun hello fel åtgärd eller testa scenariot. hello fel Analysis Service är tillståndskänslig så att den kan på ett tillförlitligt sätt kör fel och scenarier och validera resultat. Exempelvis kan en tidskrävande Testscenario köras på ett tillförlitligt sätt av hello fel Analysis Services. Och eftersom tester utförs i hello kluster, hello-tjänsten kan undersöka hello tillstånd hello-klustret och dina tjänster tooprovide mer detaljerad information om misslyckade.

## <a name="testing-distributed-systems"></a>Testa distribuerade system
Service Fabric gör hello arbetet med att skriva och hantera distribuerade skalbara program betydligt enklare. hello fel Analysis Service gör att testningen ett distribuerat program på samma sätt enklare. Det finns tre huvudsakliga problem som behöver lösas vid testning toobe:

1. Simulera/genererar fel som kan uppstå i verkliga scenarier: en hello viktiga aspekter av Service Fabric är att du kan toorecover för distribuerade program från olika fel. Dock tootest som hello program är kan toorecover från dessa fel, behöver vi en mekanism toosimulate/generera dessa verkliga fel i en kontrollerad testmiljö.
2. hello möjlighet toogenerate korrelerade fel: grundläggande fel i hello system, till exempel nätverksfel och datorn fel har enkla tooproduce individuellt. Genererar ett stort antal scenarier som kan inträffa i hello verkliga världen på grund av hello interaktioner dessa enskilda fel är icke-trivial.
3. Enhetlig miljö på olika nivåer av utveckling och distribution: det finns många fel injection system som kan utföra olika typer av fel. Hello upplevelse i alla dessa är dålig när du flyttar från en utvecklare scenarier, toorunning hello samma tester i stora testmiljöer, toousing dem för tester i produktion.

Det finns många metoder toosolve problemen, ett system som hello samma med nödvändiga garantier--alla hello sätt från en miljö med en ruta developer saknas tootest i driftskluster--. hello fel Analysis Service hjälper hello programutvecklare koncentrera dig på Testa sina affärslogik. hello fel Analysis Services innehåller alla funktioner och hello behövs tootest hello interaktion för hello-tjänsten med hello underliggande distribuerade system.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Simulera/genererar verkliga scenarier
tootest hello stabilitet i ett distribuerat system mot fel, behöver vi en mekanism toogenerate fel. I teorin genererar ett fel som en nod nedåt verkar enkelt startar träffa hello samma uppsättning konsekvenskontroll problem Service Fabric försöker toosolve. Som exempel, om vi vill tooshut ned en nod krävs hello arbetsflödet är hello följande:

1. Skicka en begäran om avstängning nod hello-klient.
2. Skicka hello begäran toohello rätt nod.
   
    a. Om hello noden inte kan hittas, bör det inte.
   
    b. Om noden hello hittas ska den returnera endast om hello-noden har stängts.

tooverify hello fel från ett test-perspektiv, hello test måste tooknow att när det här felet framkallas hello fel i själva verket inträffar. hello garanti som tillhandahåller Service Fabric är att antingen hello nod kommer kraschar eller var redan ned när hello kommandot uppnåtts hello-nod. I antingen case hello bör testa kan toocorrectly anledning om hello tillstånd och lyckas eller misslyckas korrekt i dess validering. Ett system som genomförs utanför Service Fabric toodo hello samma uppsättning fel gick träffar många nätverk, maskinvara och programvaruproblem som skulle kunna hindra den från att tillhandahålla hello föregående garantier. I hello förekomst av hello problem anges innan Service Fabric kommer omkonfigurera hello klustret tillstånd toowork runt hello problem och därför hello fel Analysis Service fortfarande anges kan toogive hello rätt garantier.

### <a name="generating-required-events-and-scenarios"></a>Skapa nödvändiga händelser och scenarier
Simulera ett verkligt fel konsekvent är svåra toostart med, hello möjlighet toogenerate korrelerade fel är även strängare. Förlust av data sker i en tillståndskänslig beständiga tjänsten när hello följande saker händer:

1. Endast ett kvorum av hello repliker som gäller skrivåtgärder fångas replikering. Alla hello sekundära repliker lag bakom hello primära.
2. hello skrivåtgärder kvorum stängs av på grund av hello repliker gå (förfaller tooa kodpaketet eller noden gå).
3. hello skrivåtgärder kvorum kan inte gå tillbaka eftersom hello data för hello repliker går förlorad (på grund av toodisk skadas eller återställning av avbildning datorn).

Dessa korrelerade fel inträffa i hello verkliga världen men inte lika ofta som enskilda kodfel. Det är viktigt att hello möjlighet tootest för dessa scenarier innan de inträffar i produktion. Även viktigare är hello möjlighet toosimulate dessa scenarier med produktionsarbetsbelastningar under övervakning (i hello mitten av hello dag med alla tekniker på däcket). Mycket bättre än att det kan bero på hello första gången i produktion klockan 02:00

### <a name="unified-experience-across-different-environments"></a>Enhetlig miljö mellan olika miljöer
hello praxis har traditionellt toocreate tre olika uppsättningar av upplevelser, ett för hello utvecklingsmiljö, en för tester och en för produktion. hello modellen var:

1. Generera tillståndsövergångar som gör kontroller av enskilda metoder i hello utvecklingsmiljö.
2. Generera fel tooallow slutpunkt till slutpunkt tester som utövar olika scenarier i hello testmiljö.
3. Behåll hello produktion miljö enkla tooprevent alla icke naturliga fel och att det är mycket snabbt mänsklig svar toofailure tooensure.

I Service Fabric via hello fel Analysis Service vi avser tooturn detta och Använd hello samma metoder från utvecklare miljö tooproduction. Det finns två sätt tooachieve detta:

1. tooinduce styrs fel, Använd hello fel Analysis Service API: er från en miljö med en ruta alla hello sätt tooproduction kluster.
2. toogive hello kluster en skall som orsakar automatisk induktion fel, Använd hello fel Analysis Service toogenerate automatisk fel. Kontrollera hello Felfrekvens via konfiguration kan hello samma tjänst toobe testas på olika sätt i olika miljöer.

Med Service Fabric trots hello skala fel skulle vara annorlunda i olika miljöer med hello skulle hello faktiska mekanismer vara identiska. Detta ger en mycket snabbare kod till distribution pipeline och hello möjlighet tootest hello services under verkliga belastning.

## <a name="using-hello-fault-analysis-service"></a>Med hjälp av hello fel Analysis Service
**C#**

Fel Analysis Service-funktionerna är i hello System.Fabric namnrymd i hello Microsoft.ServiceFabric NuGet-paketet. toouse hello fel Analysis Services-funktioner, innehålla hello nuget-paketet som en referens i projektet.

**PowerShell**

toouse PowerShell, måste du installera hello Service Fabric-SDK. Efter hello SDK är installerat hello ServiceFabric PowerShell är modulen laddas för du toouse automatiskt.

## <a name="next-steps"></a>Nästa steg
toocreate verkligen molnskala tjänster, det är kritiska tooensure både före och efter distributionen kan tjänster klarar verkligt fel. I tjänster hälsningsmeddelande idag, hello möjlighet tooinnovate snabbt och flytta koden tooproduction snabbt är mycket viktigt. hello fel Analysis Service hjälper service utvecklare toodo exakt som.

Börja testa dina program och tjänster med hjälp av inbyggda hello [testa scenarier](service-fabric-testability-scenarios.md), eller skapa egna testscenarier med hjälp av hello [fault åtgärder](service-fabric-testability-actions.md) tillhandahålls av hello fel Analysis Services.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png
