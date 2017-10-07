---
title: aaaArchitecture av Azure Service Fabric | Microsoft Docs
description: "Service Fabric är en plattform för distribuerade system används toobuild skalbar, tillförlitlig och enkelt hanteras program för hello molnet. Den här artikeln visar hello arkitektur för Service Fabric."
services: service-fabric
documentationcenter: .net
author: rishirsinha
manager: timlt
editor: rishirsinha
ms.assetid: 6b554243-70cb-4c22-9b28-1a8b4703f45e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: rsinha
ms.openlocfilehash: 0268578094ad1a0010ef44ed940f828b985f6c40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-architecture"></a>Arkitektur för Service Fabric
Service Fabric bygger med överlappande undersystem. Dessa delsystem aktivera toowrite program som är:

* Hög tillgänglighet
* Skalbar
* Hanterbara
* Kan testas

hello följande diagram visar hello viktiga undersystem för Service Fabric.

![Diagram över Service Fabric-arkitektur](media/service-fabric-architecture/service-fabric-architecture.png)

I ett distribuerat system hello möjlighet toosecurely kommunicera mellan noder i ett kluster är avgörande. Vid hello är basen för hello stack hello transport undersystem som tillhandahåller säker kommunikation mellan noder. Undersystemet ligger ovanför hello transport hello federation undersystemet, vilka kluster hello olika noder i en enda entitet (kallas kluster) så att Service Fabric kan identifiera fel, utföra ledande val och ger konsekvent routning. hello tillförlitlighet-undersystemet lager ovanpå hello federation undersystemet ansvarar för hello tillförlitlighet för Service Fabric-tjänster genom mekanismer som replikering, resurshantering och växling vid fel. hello federation undersystemet för också hello värd och aktivering undersystemet hanterar hello livscykeln för ett program på en enda nod. hello undersystem för nätverkshantering hanterar hello livscykeln för program och tjänster. hello datatillgång undersystemet hjälper programutvecklare testa sina tjänster via simulerade fel före och efter distribution av program och tjänster tooproduction miljöer. Service Fabric ger hello möjlighet tooresolve service-platser via dess undersystemet för kommunikation. Hej programmet programmeringsmodeller exponerade toodevelopers läggs ovanpå dessa delsystem tillsammans med hello programmet modellen tooenable verktygsuppsättning.

## <a name="transport-subsystem"></a>Transport-undersystemet
hello transport undersystemet implementerar en point-to-point datagram kommunikationskanal. Den här kanalen används för kommunikation inom service fabric-kluster och kommunikation mellan hello service fabric-kluster och klienter. Den stöder enkelriktade och request-reply-kommunikationsmönster som ger hello grunden för att implementera broadcast och multicast i hello Federation lager. hello transport undersystemet skyddar kommunikation med hjälp av X509 certifikat eller Windows-säkerhet. Den här undersystemet används internt av Service Fabric och är inte tillgänglig direkt toodevelopers för programmering.

## <a name="federation-subsystem"></a>Undersystemet för Federation
I ordning tooreason om en uppsättning noder i ett distribuerat system behöver du toohave en konsekvent överblick över hello system. hello federation undersystemet använder hello kommunikation primitiver tillhandahålls av hello transport undersystemet maskor hello olika noder i ett enda enhetlig kluster som den kan skäl om. Det ger hello distribuerade system primitiver behövs av hello andra delsystem - felidentifiering ledande val och konsekvent routning. hello federation undersystemet bygger på distribuerade hash-tabeller med en 128-bitars token utrymme. hello undersystemet skapar en ringtopologi över hello noder, med varje nod i hello ring som har tilldelats en delmängd av hello token utrymme för ägarskap. För felidentifiering använder hello layer en leasingmekanism baserat på hjärta beating och skiljedom. hello federation undersystemet garanterar även via invecklade koppling och avvikelse protokoll som en enda ägare av en token finns när som helst. Detta ger ledande val och konsekvent routning garantier.

## <a name="reliability-subsystem"></a>Tillförlitlighet undersystemet
hello tillförlitlighet undersystemet innehåller hello mekanism toomake hello tillståndet för en Service Fabric-tjänsten med hög tillgänglighet via hello hello *replikatorn*, *Failover Manager*, och  *Resursutjämnaren*.

* hello replikatorn garanterar tillståndsändringar i hello primära tjänsten replik kommer automatiskt att replikerade toosecondary repliker, underhålla konsekvensen mellan hello primära och sekundära repliker i en replikuppsättning för tjänsten. hello replikatorn är ansvarig för kvorumhantering mellan hello repliker i hello replikuppsättningen. Den samverkar med hello failover unit tooget hello lista över operations tooreplicate och hello omkonfigurationsagent ger den hello configuration hello replikuppsättningen. Denna konfiguration anger vilka repliker hello-åtgärder måste toobe replikeras. Service Fabric tillhandahåller en standard replikatorn kallas Fabric replikatorn som kan användas av hello programming modellen API toomake hello Tjänststatus hög tillgänglighet och tillförlitlig.
* hello Failover Manager säkerställer att när noder läggs tooor bort från hello kluster, hello distribueras belastningen över hello tillgängliga noder. Om en nod i klustret hello misslyckas hello klustret automatiskt att konfigurera om hello tjänsttillgänglighet repliker toomaintain.
* hello Resource Manager placerar service repliker över fel domän i hello kluster och ser till att alla redundansenheter är fungerar. hello Resource Manager balanserar också serviceresurser över hello underliggande delad pool av klustret noder tooachieve optimala uniform belastningsdistribution.

## <a name="management-subsystem"></a>Undersystem för nätverkshantering
hello undersystem för nätverkshantering ger slutpunkt till slutpunkt-tjänsten och Livscykelhantering för programmet. PowerShell-cmdlets och administrativa API: er kan du tooprovision, distribuera, korrigering, uppgraderar och avetablera program utan förlust av tillgänglighet. hello undersystem för nätverkshantering utför via hello följande tjänster.

* **Klusterhanterare**: Detta är primär hello-tjänst som interagerar med hello Failover Manager från tillförlitlighet tooplace hello program på hello noder baserat på hello placeringsbegränsningar för tjänsten. hello Resource Manager i redundans undersystemet säkerställer att hello begränsningar aldrig är bruten. hello Klusterhanterare hanterar hello livscykeln för hello program från etablera toode-provision. Den kan integreras med hello health manager tooensure att programmets tillgänglighet inte är förlorade ur semantiska hälsa under uppgraderingar.
* **Hälsoindikatorn**: den här tjänsten kan hälsoövervakning av program, tjänster och klustret entiteter. Kluster-enheter (till exempel noder, partitioner för tjänsten och repliker) kan rapportera hälsoinformation sedan samman till hello centraliserad health store. Den här hälsoinformation ger en övergripande i tidpunkt hälsa ögonblicksbild av hello tjänster och noder som distribueras över flera noder i hello kluster, vilket gör att du tootake eventuella nödvändiga korrigerande åtgärder. Hälsotillstånd frågan API: er kan du tooquery hello hälsa händelser rapporterade toohello hälsa undersystemet. hello hälsa API: er returneras hello rådata hälsa data som lagras i hello hälsa lagrar eller hello aggregeras, tolkas hälsa data för ett specifikt kluster-enheten.
* **Image Store**: den här tjänsten tillhandahåller lagring och distribution av hello binärfiler. Den här tjänsten tillhandahåller en enkel distribuerade lagring där hello program är överförda tooand som hämtats från.

## <a name="hosting-subsystem"></a>Värd för undersystemet
hello Klusterhanterare informerar hello som värd för undersystemet (som körs på varje nod) som underhåller den behöver toomanage för en viss nod. hello värd undersystemet sedan hanterar hello livscykeln för hello program på noden. Den samverkar med hello tillförlitlighet komponenter tooensure att hello repliker är på rätt plats och är felfria.

## <a name="communication-subsystem"></a>Undersystemet för kommunikation
Den här undersystemet erbjuder tillförlitlig meddelandehantering inom hello klustret och service discovery via hello Naming service. hello Naming service matchar namnen tooa plats i hello klustret och gör att användare toomanage Tjänstenamn och egenskaper. Med hello Naming service kan klienter på ett säkert sätt kommunicera med en nod i hello klustret tooresolve ett tjänstnamn och hämta service metadata. Med en enkel Naming klient API kan utveckla användare av Service Fabric tjänster och klienter som kan lösa hello aktuella nätverksplats trots nod dynamik eller hello storlek igen hello-klustret.

## <a name="testability-subsystem"></a>Möjlighet att testa undersystemet
Möjlighet att testa är en uppsättning med verktyg som utformats för att testa tjänster som bygger på Service Fabric. hello verktyg kan utvecklare enkelt medföra meningsfulla fel och köra testet scenarier tooexercise och validera hello många tillstånd och övergångar som en tjänst får alla på ett sätt som kontrollerade och säkra under hela sin livslängd. Möjlighet att testa innehåller också en mekanism toorun längre tester som kan gå igenom olika möjliga fel utan att förlora tillgänglighet. Detta ger dig testa i produktionsmiljö.

