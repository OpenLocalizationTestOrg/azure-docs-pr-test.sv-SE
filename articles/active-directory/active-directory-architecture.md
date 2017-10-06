---
title: aaaUnderstand Azure Active Directory-arkitektur | Microsoft Docs
description: "Förklarar vad en Azure AD-klient är och hur toomanage Azure via Azure Active Directory"
services: active-directory
documentationcenter: 
author: MarkusVi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2017
ms.author: markvi
ms.openlocfilehash: 799943c012dcc309907ed3c36372038a0aad222a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-active-directory-architecture"></a>Förstå Azure Active Directory-arkitekturen
Azure Active Directory (AD Azure) aktiverar du toosecurely hantera tooAzure tjänster och resurser för dina användare. En komplett uppsättning identitetshanteringsfunktioner ingår i Azure AD. Information om funktionerna i Azure AD finns i [Vad är Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis)

Med Azure AD kan du skapa och hantera användare och grupper och aktivera behörigheter tooallow och neka tooenterprise resurser. Information om Identitetshantering finns [hello grunderna i Azure Identitetshantering](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity).

## <a name="azure-ad-architecture"></a>Azure AD-arkitekturen
Azure AD geografiskt distribuerad arkitektur kombinerar omfattande övervakning, automatisk omdirigering, redundans och återställningsfunktioner aktivera oss toodeliver på företagsnivå tillgänglighet och prestanda tooour kunder.

hello följande arkitektur elementen beskrivs i den här artikeln:
 *  Tjänstarkitekturens design
 *  Skalbarhet 
 *  Kontinuerlig tillgänglighet
 *  Datacenter

### <a name="service-architecture-design"></a>Tjänstarkitekturens design
hello vanligaste sättet toobuild en skalbar, hög tillgänglighet och dataintensiva systemet är via oberoende byggblock eller skalningsenheter för datanivå hello Azure AD, skalenheter kallas *partitioner*. 

hello datanivå har flera frontend-tjänster som ger Läs-och skrivbehörighet kapaciteten. hello diagrammet nedan visar hur hello komponenter av en enskild katalogpartition distribueras i geografiskt distrubuted datacenter. 

  ![Partitioner med en enda katalog](./media/active-directory-architecture/active-directory-architecture.png)

hello komponenter i Azure AD-arkitekturen är en primär replik och sekundära replikerna.

**Primär replik**

Hej *primära repliken* tar emot alla *skriver* för hello-partition som den tillhör. Någon skriva åtgärden är omedelbart replikerade tooa sekundär replik i olika datacenter innan det returneras lyckade toohello anroparen, tillse geo-redundant hållbarhet för skrivningar.

**Sekundära repliker**

Alla *katalogläsningar* hanteras från *sekundära repliker*. Sekundära repliker är datacenter som är fysiskt placerade i olika geografiska områden. Det finns många sekundära repliker eftersom data replikeras asynkront. Directory läser, till exempel autentiseringsbegäranden, hanteras från datacenter som är nära tooour kunder. hello sekundära repliker är ansvarig för skrivskyddade skalbarhet.

### <a name="scalability"></a>Skalbarhet

Skalbarhet är hello möjligheten för en tjänst tooexpand toomeet öka krav på prestanda. Skriva skalbarhet uppnås genom partitionering hello data. Läs skalbarhet uppnås genom att replikera data från en partition toomultiple sekundära repliker distribueras i hälsningsmeddelande.

Begäranden från katalogen program är vanligtvis routade toohello datacenter som de är fysiskt närmast. Skrivningar är transparent omdirigerade toohello primära repliken tooprovide skrivskyddad konsekvenskontroll. Sekundära repliker utöka avsevärt hello skalan för partitioner eftersom hello kataloger normalt fungerar läser de flesta hello tid.

Katalogen program ansluta toohello närmsta datacenter. Detta förbättrar prestanda vilket gör det möjligt att skala upp. Eftersom en katalogpartition kan ha många sekundära repliker, kan sekundära repliker placeras närmare toohello directory-klienter. Endast internt directory service komponenter som är write-intensiva mål hello active primära repliken direkt.

### <a name="continuous-availability"></a>Kontinuerlig tillgänglighet

Definierar hello möjligheten för en system tooperform oavbruten tillgänglighet (eller drifttid). hello viktiga tooAzure Annonsens hög tillgänglighet är att våra tjänster snabbt kan flytta trafik över flera geografiskt spridd datacenter. Alla datacenter är oberoende av varandra, vilket innebär att fellägen kan hållas åtskilda.

Azure AD är-partitionen förenklad jämfört med toohello enterprise AD-design, vilket är nödvändigt för att skala upp hello system. Vi har implementerat en design med en enskild huvudreplik och en noggrant styrd och deterministisk redundans mellan repliker.

**Feltolerans**

Ett system finns mer om det är feltolerant toohardware-, nätverks- och programvara fel. För varje partition på hello katalog, en hög tillgänglighet master replik finns: hello primära repliken. Endast skrivningar toohello partition utförs på repliken. Repliken är kontinuerligt och noggrant övervakas och skrivningar kan vara omedelbart flyttats pekar tooanother repliken (som blir hello nya primära) om ett fel upptäcks. Under en redundansväxling kan det uppstå ett avbrott i skrivtillgängligheten på 1 till 2 minuter. Lästillgängligheten påverkas inte under den här tiden.

Läsåtgärder (som outnumber skrivningar av många i storlek) bara gå toosecondary repliker. Eftersom sekundära repliker är idempotent kan kompenseras enkelt förlust av alla en replik i en given partition genom att dirigera hello läsningar tooanother repliken, vanligtvis i hello samma datacenter.

**Datahållbarhet**

En skrivning är varaktigt allokerat tooat minst två data Datacenter tidigare tooit som ska bekräftas. Detta sker genom att först utför hello skrivning vid hello primära och sedan omedelbart replikerar hello skrivåtgärder tooat minst en andra datacenter. Detta säkerställer att en potentiell oåterkallelig förlust av hello data center värd hello primära inte leder till dataförlust.

Azure AD har noll [Recovery tid mål för Återställningstid](https://en.wikipedia.org/wiki/Recovery_time_objective) för utfärdande och directory läser och hello efter minuter (~ 5 minuter) RTO för katalogen skriver. [Målet för återställningspunkter (RPO)](https://en.wikipedia.org/wiki/Recovery_point_objective) är också noll, och inga data går förlorade vid redundansväxlingar.

### <a name="data-centers"></a>Datacenter

Azure AD replikerna lagras i finns i hela hello world-datacenter. Mer information finns i avsnittet om [Azure-datacenter](https://azure.microsoft.com/en-us/overview/datacenters).

Azure AD fungerar mellan datacenter med hello följande egenskaper:

 * Autentisering, diagram och andra AD-tjänster bakom hello Gateway-tjänsten. hello Gateway hanterar belastningsutjämning för dessa tjänster. Den växlar automatiskt över om servrar med feltillstånd identifieras under transaktionella hälsotillståndsavsökningar. Baserat på dessa hälsoavsökningar dirigerar hello Gateway dynamiskt trafik toohealthy datacenter.
 * För *läser*, hello katalogen har sekundära repliker och motsvarande frontend-tjänster i en aktiv-aktiv konfiguration i flera datacenter. Om ett fel uppstår på ett helt Datacenter blir trafik automatiskt routade tooa olika datacenter.
 *  För *skriver*, hello directory kommer redundans primära (master) repliken mellan Datacenter via planerad (nya primära är synkroniserade tooold primära) eller nödfall redundans procedurer. Data hållbarhet uppnås genom att replikera alla commit tooat minst två datacenter.

**Datakonsekvens**

hello directory modellen är en av slutliga konsekvensen. En typisk problemet med distribuerade asynkront replikering system är hello data som returneras från en ”viss” repliken inte kanske upp toodate. 

Azure AD innehåller skrivskyddad konsekvens för tillämpningar för en sekundär replik av Routning skrivningar toohello primära repliken och dra synkront hello skriver tillbaka toohello sekundär replik.

Programmet skriver med hello Graph API för Azure AD är tas ut från underhålla tillhörighet tooa directory repliken konsekvent skrivskyddad. hello Azure AD Graph tjänsten upprätthåller en logisk session som har tillhörighet tooa sekundär replik för läsningar; tillhörighet fångas i ett ”replik-token” som hello diagrammet tjänsten cache med hjälp av en distribuerad cache. Den här variabeln används sedan för efterföljande åtgärder i hello samma logiska session. 

 >[!NOTE]
 >Skrivningar replikeras direkt toohello sekundär replik toowhich hello logiska sessionens läsningar har utfärdats.
 >

**Skydd av säkerhetskopior**

hello directory implementerar mjuka borttagningar i stället för hårda borttagningar för användare och enkel återställning av oavsiktliga borttagningar av en kund-klienter. Om administratören innehavare av misstag tar bort användare, kan de enkelt ångra och återställa hello bort användare. 

Azure AD implementerar dagliga säkerhetskopieringar av alla data och kan därför auktoritativt återställa data i händelse av logiska borttagningar eller skadade data. Vår datanivå använder felkorrigeringskoder för felkontroll och automatisk korrigering av särskilda typer av diskfel.

**Mätvärden och övervakare**

Körning av en tjänst med hög tillgänglighet kräver förstklassiga mät- och övervakningsfunktioner. Azure AD analyserar och rapporterar kontinuerligt viktiga mätvärden och framgångskriterier rörande tjänsternas hälsa för var och en av dess tjänster. Vi utvecklar och finjusterar kontinuerligt mätvärden, övervakning och aviseringar för alla scenarier i varje Azure AD-tjänst och för alla tjänster.

Om Azure AD-tjänsten inte fungerar som förväntat, vidta vi omedelbart åtgärden toorestore funktioner så snabbt som möjligt. hello viktigaste mått Azure AD-spårar är hur snabbt kan identifiera och minska en kund eller live problem. Vi investera kraftigt i övervakning och aviseringar toominimize tid toodetect (TTD mål: < 5 minuter) och Operativ beredskap toominimize tid toomitigate (TTM mål: < 30 minuter).

**Säkra åtgärder**

Vi använder operativa kontroller, till exempel multifaktorautentisering (MFA) för alla åtgärder, samt granskning av alla åtgärder. Dessutom kan använder vi en just-in-time-höjning system toogrant tillfällig åtkomst som krävs för alla operativa uppgiften på begäran med jämna mellanrum. Mer information finns i [hello betrodda molnet](https://azure.microsoft.com/en-us/support/trust-center).

## <a name="next-steps"></a>Nästa steg
[Utvecklarhandbok för Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-developers-guide)

