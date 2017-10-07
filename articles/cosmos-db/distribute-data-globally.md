---
title: aaaDistribute data globalt med Azure Cosmos DB | Microsoft Docs
description: "Läs mer om planeten skala geo-replikering, redundans och data återställning med hjälp av globala databaser från Azure Cosmos DB, ett globalt distribuerade kommer mutli-modell database-tjänsten."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: ba5ad0cc-aa1f-4f40-aee9-3364af070725
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/14/2017
ms.author: arramac
ms.openlocfilehash: b50e8433dc7e70c54d68c4c2f99954a13f4951f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodistribute-data-globally-with-azure-cosmos-db"></a>Hur toodistribute data globalt med Azure Cosmos DB
Azure är allt vanligare - den har en global storleken över 30 + geografiska regioner och kontinuerligt växer. Med dess världen är en hello differentierade funktioner som Azure erbjuder tooits utvecklare hello möjlighet toobuild, distribuera och hantera globalt distribuerade program enkelt. 

[Azure Cosmos DB](../cosmos-db/introduction.md) är Microsofts globalt distribuerade databastjänst för flera datamodeller för verksamhetskritiska program. Azure Cosmos-DB tillhandahåller NYCKELFÄRDIGT global distributionsplatsen [elastisk skalbarhet av dataflöden och lagringsutrymmen](../cosmos-db/partition-data.md) över hela världen, en siffra millisekunders latens vid hello 99th percentilen, [fem väldefinierade konsekvensnivåer ](consistency-levels.md), och garanteras hög tillgänglighet, alla säkerhetskopieras av [branschledande serviceavtal](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos-DB [indexerar automatiskt data](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) utan att du toodeal med hantering av schemat och index. Det stöder flera modeller och dokument, nyckelvärde graf och kolumndatamodeller. Som en moln-föddes tjänst framtagen Azure Cosmos DB noggrant med multitenans och global distributionsplatsen från hello bakgrund.

**En enda Azure DB som Cosmos-samling partitioneras och distribuerade över flera Azure-regioner**

![Azure DB Cosmos-samling partitioneras och distribuerade i tre områden](./media/distribute-data-globally/global-apps.png)

Vi har lärt dig när du skapar Azure Cosmos DB lägger till global distributionsplatsen inte vara efterhand - det får inte vara ”bult på-ovanpå ett” plats ”-databassystem. hello-funktionerna som erbjuds av en globalt distribuerad databas som omfattar utöver att av traditionella geografiska disaster recovery (Geo DR) som erbjuder ”enskild plats”-databaser. Plats-databaser som erbjuder Geo DR-funktionen är en strikt uppsättning globalt distribuerade databaser. 

Med Azure Cosmos DB NYCKELFÄRDIGT distributionslistor utvecklare har inte toobuild sina egna scaffold-teknik för replikering genom att använda antingen hello Lambda-mönster (till exempel [AWS DynamoDB replikering](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) via hello databasloggen eller av gör ”dubbla skrivningar” över flera regioner. Vi inte rekommenderar metoderna eftersom det är omöjligt tooensure är korrekt av dessa metoder och ange ljud SLA: er. 

Vi ger en översikt över Azure Cosmos DB global distributionsplatsen funktioner i den här artikeln. Dessutom beskrivs Azure Cosmos DB unika tillvägagångssätt tooproviding omfattande SLA: er. 

## <a id="EnableGlobalDistribution"></a>Aktivera NYCKELFÄRDIGT global distributionsplatsen
Azure Cosmos-DB tillhandahåller hello följande funktioner tooenable du tooeasily skriva planeten skala program. Dessa funktioner är tillgängliga via hello Azure Cosmos DB'S resource provider-baserad [REST API: er](https://docs.microsoft.com/rest/api/documentdbresourceprovider/) samt hello Azure-portalen.

### <a id="RegionalPresence"></a>Allt vanligare regionala förekomst 
Azure ständigt växande geografiska sig genom att ta [nya områden](https://azure.microsoft.com/regions/) online. Azure Cosmos-databasen är tillgänglig i alla nya Azure-regioner som standard. Detta ger dig tooassociate en geografisk region med din Azure Cosmos DB databaskonto som Azure öppnar hello nytt område för företag.

**Azure Cosmos-databasen är tillgänglig i alla Azure-regioner som standard**

![Azure Cosmos DB tillgängliga på alla Azure-regioner](./media/distribute-data-globally/azure-regions.png)

### <a id="UnlimitedRegionsPerAccount"></a>Associera ett obegränsat antal regioner med din Azure DB som Cosmos-databaskonto
Azure Cosmos-DB kan du tooassociate valfritt antal Azure-regioner med Azure Cosmos-DB databasen konto. Utanför geobegränsning begränsningar (till exempel Kina, Tyskland) finns det inga begränsningar för hello antalet regioner som kan vara kopplad till ditt konto för Azure DB som Cosmos-databasen. hello följande bild visar ett konto som är konfigurerat toospan för databasen i 25 Azure-regioner.  

**En klients Azure Cosmos DB databasen konto spanning 25 Azure-regioner**

![Azure DB Cosmos-databaskonto utsträckning 25 Azure-regioner](./media/distribute-data-globally/spanning-regions.png)

### <a id="PolicyBasedGeoFencing"></a>Principbaserad geobegränsning
Azure Cosmos-DB är utformad toohave principbaserad geobegränsning funktioner. Geobegränsning är en viktig komponent tooensure databegränsningar för styrning och efterlevnad och förhindra att associera en viss region med ditt konto. Exempel på geobegränsning inkluderar (men inte är begränsade till) scope global distributionsplatsen toohello regioner inom ett suveräna moln (till exempel Kina och Tyskland) eller i ett government skatt (till exempel Australien). hello principer styrs med hello metadata för din Azure-prenumeration.

### <a id="DynamicallyAddRegions"></a>Dynamiskt lägga till och ta bort områden
Azure Cosmos-DB kan du tooadd (koppla) eller ta bort (koppla bort) regioner tooyour databaskonto vid någon tidpunkt (se [föregående bild](#UnlimitedRegionsPerAccount)). Tack vare replikering av data mellan partitioner parallellt, garanterar Azure Cosmos DB att när en ny region är online, Azure Cosmos DB är tillgänglig inom 30 minuter var som helst i hälsningsmeddelande för in too100 TBs. 

### <a id="FailoverPriorities"></a>Prioriteringar för växling vid fel
möjligt tooassociate hello prioritet toovarious regioner som är associerade med hello databaskonto toocontrol exakt i vilken ordning regional växling vid fel när det finns flera regioner avbrott Azure Cosmos DB (se följande figur hello). Azure Cosmos-DB säkerställer att hello automatisk redundans inträffar i hello prioritetsordning som du angav. Mer information om regional växling vid fel finns [automatisk regional växling vid fel för kontinuitet i Azure Cosmos DB](regional-failover.md).

**En klient i Azure Cosmos DB kan konfigurera hello prioritetsordning för växling vid fel (höger) för områden som är associerad med ett databaskonto**

![Konfigurera redundans prioriteter med Azure Cosmos DB](./media/distribute-data-globally/failover-priorities.png)

### <a id="OfflineRegions"></a>Dynamiskt koppla en region ””
Azure Cosmos-DB kan tootake databasen kontot offline i en viss region och ta den online igen senare. Regioner som markerats offline delta inte aktivt i replikering och är inte en del av hello redundans sekvens. Detta gör att du toofreeze hello senast kända fungerande databas bilden i något av hello Läs regioner innan lansera kan vara riskabelt uppgraderar tooyour program.

### <a id="ConsistencyLevels"></a>Flera väldefinierade konsekvenskontroll modeller för globalt replikerade databaser
Azure Cosmos-DB exponerar [flera väldefinierade konsekvensnivåer](consistency-levels.md) backas upp av SLA: er. Du kan välja en specifik konsekvent modell (hello tillgänglig lista med alternativ) beroende på hello arbetsbelastning/scenarier. 

### <a id="TunableConsistency"></a>Justerbara konsekvens för globalt replikerade databaser
Azure Cosmos-DB kan du tooprogrammatically åsidosättning och minska hello konsekvenskontroll standardalternativet på grundval per begäran vid körning. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dynamiskt konfigurerbara Läs- och skrivåtgärder regioner
Azure Cosmos-DB möjliggör tooconfigure hello regioner (associerad med hello databasen) för ”läsa”, ”write” eller ”läsa/skriva” regioner. 

### <a id="ElasticallyScaleThroughput"></a>Elastiskt skalning genomströmning över Azure-regioner
Du kan Elastiskt skala en samling Azure Cosmos DB genom etablering genomströmning programmässigt. hello dataflödet är tillämpade tooall hello regioner hello samling distribueras i.

### <a id="GeoLocalReadsAndWrites"></a>GEO-lokala läser och skriver
hello viktigaste fördelen för ett globalt distribuerad databas är toooffer låg latens åtkomst toohello data var som helst i hälsningsmeddelande. Azure Cosmos-DB erbjuder låg latens garantier på P99 för olika databasåtgärder. Det säkerställer att alla Läs är routade toohello närmaste lokala skrivskyddade region. tooserve en läsbegäran hello kvorum lokala toohello region där hello läsa utfärdas används. hello gäller samma toohello skrivningar. En skrivning bekräftas endast när en majoritet av replikerna begått varaktigt hello skrivåtgärder lokalt men utan att gated på fjärranslutna repliker tooacknowledge hello skrivningar. Placera olika fungerar hello replikering protokollet för Azure Cosmos DB hello antagande som hello läsa och skriva beslutsförhet är alltid lokala toohello läsa och skriva regioner, i vilken hello begäran gjordes.

### <a id="ManualFailover"></a>Manuellt initiera regional växling vid fel
Azure Cosmos-DB kan du tootrigger hello redundans för hello databasen konto toovalidate hello *avslutas tooend* tillgänglighet egenskaper för hela hello-program (utöver hello databas). Eftersom både hello säkerhet och liveness egenskaper för hello fel identifiering och ledande val är garanterat Azure Cosmos DB garanterar *noll dataförlust* för en klient-initierad manuell redundansväxling.

### <a id="AutomaticFailover"></a>Automatisk redundans
Azure Cosmos-DB stöder automatisk redundans om en eller flera regionala avbrott. Under en regional växling vid fel upprätthåller Azure Cosmos DB dess skrivskyddade svarstid, drifttid tillgänglighet, konsekvens och genomströmning SLA: er. Azure Cosmos-DB innehåller ett övre gränsvärde för en automatisk redundans åtgärden toocomplete hello varaktighet. Detta är hello fönstret i förlust av data under hello regionalt strömavbrott.

### <a id="GranularFailover"></a>Utformad för olika redundans granulariteter
För närvarande hello automatisk och manuell växling funktioner som exponeras vid hello Granulariteten för hello konto. Observera internt Azure Cosmos DB är utformad toooffer *automatisk* växling vid fel på ökad detaljnivå med en databas, samling eller även en partition (i en samling som äger en mängd nycklar). 

### <a id="MultiHomingAPIs"></a>Flera API: er i Azure Cosmos DB
Azure Cosmos-DB kan du toointeract med hello-databas med hjälp av antingen logiska (region oberoende) eller fysisk (regionspecifika)-slutpunkter. Om du använder logiska slutpunkter säkerställer att programmet hello transparent kan vara multi-homed vid redundans. Hej senare, fysiska slutpunkter, ange finmaskig kontroll toohello programmet tooredirect läser och skriver toospecific regioner.

Du kan hitta information om hur tooconfigure läsa inställningar för hello [DocumentDB API](../cosmos-db/tutorial-global-distribution-documentdb.md), [Graph API](../cosmos-db/tutorial-global-distribution-graph.md), [tabell API](../cosmos-db/tutorial-global-distribution-table.md), och [MongoDB API](../cosmos-db/tutorial-global-distribution-mongodb.md)i deras respektive länkade artiklar.

### <a id="TransparentSchemaMigration"></a>Migrering av öppet och konsekvent databasen schemat och index 
Azure Cosmos-DB är helt [schema oberoende](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). hello unika designen av dess databasmotorn kan den tooautomatically och index synkront alla hello data den en utan att något schema eller sekundärindex från dig. Detta gör att du tooiterate globalt distribuerade program snabbt utan att oroa sig för migrering av databasen schemat och index eller koordinera med lanseringar av schemaändringar. Azure Cosmos-DB garanterar att alla ändringar tooindexing principer som du uttryckligen har gjort inte resulterar i försämring av prestanda eller tillgänglighet.  

### <a id="ComprehensiveSLAs"></a>Omfattande SLA (utöver bara hög tillgänglighet)
Som ett globalt distribuerad databas-tjänsten erbjuder Azure Cosmos DB väldefinierade SLA för **förlust av data**, **tillgänglighet**, **svarstid P99**, **genomflöde**  och **konsekvenskontroll** för hello databasen hela oavsett hello antalet områden som är associerade med hello-databas.  

## <a id="LatencyGuarantees"></a>Latens garantier
hello viktigaste fördelen för en tjänst för globalt distribuerad databas som Azure Cosmos DB är toooffer låg latens åtkomst tooyour data var som helst i hälsningsmeddelande. Azure Cosmos-DB tillhandahåller garanterad låg svarstid P99 för olika databasåtgärder. hello replikering protokoll som använder Azure Cosmos DB garanterar att hello databasåtgärder (helst både läsning och skrivning) utförs alltid i hello region lokala toothat av hello-klienten. hello latens SLA för Azure Cosmos DB innehåller P99 för både läser (synkront) indexerade skrivningar och frågor för olika storlekar på förfrågan och svar. hello latens garantier för skrivningar innehålla beständiga majoritet kvorum incheckningar inom hello lokala datacenter.

### <a id="LatencyAndConsistency"></a>Latenss relationen med konsekvenskontroll 
Den måste toosynchronously replikera hello skrivningar för ett globalt distribuerad tjänst toooffer stark konsekvens i en global inställning, eller synkron utföra mellan region läsningar – hello ljust och hello wide area network tillförlitlighet bestämmer hastighet Det stark konsekvent resulterar i hög fördröjning och låg tillgängligheten för databasåtgärder. I ordning toooffer garanteras låg latens på P99 och 99.99 tillgänglighet, därför skyddas hello tjänsten asynkron replikering. Den här i sin tur kräver att hello-tjänsten måste också erbjuda [väldefinierade, Avslappnad konsekvenskontroll choice(s)](consistency-levels.md) – svagare än starkt (toooffer låg latens och tillgänglighet garantier) och helst starkare än ”slutliga” konsekvensen ( toooffer en intuitiv programmeringsmodell).

Azure Cosmos-DB garanterar att en Läsåtgärd inte är obligatoriska toocontact repliker över flera regioner toodeliver hello specifika konsekvenskontroll nivån garanti. På samma sätt ser till att en skrivning inte hämta blockeras medan hello data replikeras mellan alla hello regioner (d.v.s. skrivningar asynkront replikeras över regioner). För flera regioner databasen konton är flera Avslappnad konsekvensnivåer tillgängliga. 

### <a id="LatencyAndAvailability"></a>Latenss relationen med tillgänglighet 
Tillgänglighet och svarstid är hello två sidor av hello samma mynt. Vi om fördröjning av hello-åtgärden i stabilt tillstånd och tillgänglighet i hello ansikte fel. Från hello programs synvinkel är en långsam körs databasåtgärd särskiljas från en databas som inte är tillgänglig. 

toodistinguish fördröjningar från otillgänglighet Azure Cosmos DB ger en absolut övre gräns på svarstiden för olika databasåtgärder. Om hello Databasåtgärden tar längre tid än hello övre gränsen toocomplete returnerar Azure Cosmos DB ett timeout-fel. hello Azure Cosmos DB tillgänglighets-SLA säkerställer att hello tidsgränser räknas mot hello tillgänglighets-SLA. 

### <a id="LatencyAndThroughput"></a>Latenss relation med genomströmning
Azure Cosmos-DB göra inte dig att välja mellan svarstid och genomströmning. Den godkänner hello SLA för båda svarstid P99 och leverera hello dataflödet som du har etablerat. 

## <a id="ConsistencyGuarantees"></a>Konsekvens garanterar
När hello [stark konsekvens modellen](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) är hello guld standard för programmering, som det gäller vid hello brant priset för hög latens (i stabilt tillstånd) och förlust av tillgänglighet (i hello framsidan fel). 

Azure Cosmos-DB erbjuder en väldefinierad programming modellen tooyou tooreason om replikerade datakonsekvens. Ordna tooenable du toobuild multi-homed program, hello konsekvenskontroll modeller som exponeras av Azure Cosmos DB är utformad toobe region-oberoende och inte är beroende av hello region från där hello läsningar och skrivningar hanteras. 

SERVICENIVÅAVTAL för Azure Cosmos DB konsekvens garanterar att 100% för läsbegäranden uppfyller hello konsekvenskontroll garanti för hello konsekvensnivå som begärs av du antingen (hello konsekvenskontroll Standardnivå på hello databaskonto eller hello åsidosätts värdet på hello begäran ). En läsbegäran anses toohave uppfyllda hello konsekvenskontroll SLA om alla hello konsekvens garanterar som är associerade med hello konsekvensnivå är uppfyllda. hello nedan samlar in hello konsekvens garanterar som motsvarar toospecific konsekvensnivåer som erbjuds av Azure Cosmos DB.

**Konsekvens garanterar som är associerade med en viss konsekvensnivå i Azure Cosmos DB**

<table>
    <tr>
        <td><strong>Konsekvensnivå</strong></td>
        <td><strong>Egenskaper för konsekvenskontroll</strong></td>
        <td><strong>SLA</strong></td>
    </tr>
    <tr>
        <td rowspan="3">Session</td>
        <td>Läsa dina egna skrivning</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Monotonisk Läs</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Konsekvent prefix</td>
        <td>100%</td>
    </tr>
    <tr>
        <td rowspan="3">Begränsad föråldrad</td>
        <td>Monotonisk läsa (inom ett område)</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Konsekvent prefix</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Föråldrad bunden &lt; K, T</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Konsekvent prefix</td>
        <td>Konsekvent prefix</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Stark</td>
        <td>Linearizable</td>
        <td>100%</td>
    </tr>
</table>

### <a id="ConsistencyAndAvailability"></a>Konsekvenskontrolls relationen med tillgänglighet
Hej [är omöjligt resultatet](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) av hello [CAP-sats](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) bevisar att den är faktiskt omöjligt för hello system tooremain tillgängliga och erbjudande linearizable konsekvens i hello ansikte fel. hello database-tjänsten måste välja toobe CP eller AP - CP system över tillgänglighet för linearizable konsekvenskontroll medan hello AP system över [linearizable konsekvenskontroll](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) för tillgänglighet. Azure Cosmos-DB aldrig överskrider hello begärt konsekvensnivå, vilket gör det formellt en CP-system. Men i praktiken konsekvenskontroll är inte en allt eller inget lösning – det finns flera väldefinierade konsekvenskontroll modeller längs hello konsekvenskontroll spektrumet mellan linearizable och eventuell konsekvenskontroll. Vi har försökt tooidentify i Azure Cosmos DB flera hello mjukas upp konsekvenskontroll modeller med verkligheten tillämplighet och en intuitiv programmeringsmodell. Azure Cosmos-DB navigerar hello konsekvenskontroll tillgänglighet kompromisser genom att erbjuda 99.99 tillgänglighet SLA tillsammans med [flera mjukas upp ännu väldefinierade konsekvensnivåer](consistency-levels.md). 

### <a id="ConsistencyAndAvailability"></a>Konsekvenskontrolls relation med en fördröjning
En mer omfattande variation av CAP föreslogs av Prof. Daniel Abadi och kallas [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), vilket även konton för fördröjning och konsekvens kompromisser i stabilt tillstånd. Det tillstånd som i stabilt tillstånd hello databassystemet måste du välja mellan konsekvens och svarstid. Azure Cosmos DB säkerställer att alla läsningar och skrivningar är lokala toohello läsa och skriva regioner respektive med flera Avslappnad konsekvenskontroll modeller (understöds av asynkron replikering och lokal läsning, skrivning beslutsförhet).  Detta gör att Azure Cosmos DB toooffer låg latens garanterar inom hello region för hello konsekvensnivåer.  

### <a id="ConsistencyAndThroughput"></a>Konsekvenskontrolls relation med genomströmning
Eftersom hello implementering av en specifik konsekvent modell beror på hello val av en [kvorum](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), hello genomströmning också varierar beroende på hello valet av konsekvenskontroll. Till exempel i Azure Cosmos DB är hello dataflöde med starkt konsekvent läsningar ungefär halva toothat överensstämmelse läsningar. 
 
**Förhållandet mellan Läs kapacitet för en specifik konsekvensnivå i Azure Cosmos DB**

![Förhållandet mellan konsekvens och dataflöde](./media/distribute-data-globally/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Genomströmning garantier 
Azure Cosmos-DB kan du tooscale genomströmning (samt lagring), Elastiskt över olika regioner, beroende på hello-begäran. 

**En enda Azure DB som Cosmos-samling partitionerad (över tre shards) och därefter distribueras över tre Azure-regioner**

![Azure Cosmos-DB distribueras och partitionerade samlingar](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

En samling Azure Cosmos DB hämtar distribueras med hjälp av två dimensioner – inom en region och sedan över regioner. Så här gör du: 

* Inom en enskild region är en samling Azure Cosmos DB skala ut vad gäller resurs partitioner. Varje resurspartition hanterar en uppsättning nycklar och är starkt konsekvent och hög tillgänglighet tack vare tillstånd replikeringen mellan en uppsättning repliker. Azure Cosmos-DB är ett fullständigt resurs styrs system där en resurspartition ansvarar för att leverera sin andel av genomströmning för system-resurser som är allokerade tooit hello budget. hello skalning av en samling Azure Cosmos DB är helt transparent – Azure Cosmos DB hanterar hello resurs partitioner och delar upp och slår ihop den efter behov. 
* Var och en av hello resurs partitioner distribueras sedan över flera regioner. Resursen partitioner äger hello samma uppsättning nycklar över olika regioner formuläret partitionsuppsättningen (se [föregående bild](#ThroughputGuarantees)).  Resursen partitioner i en partition är samordnad med hjälp av tillstånd replikeringen mellan hello flera regioner. Beroende på hello konsekvensnivå har konfigurerats, konfigureras hello resurs partitioner i en partition dynamiskt med olika topologier (till exempel star, sammankopplad kedja, träd osv.). 

Tack vare en mycket dynamisk partition hantering, belastningsutjämning och strikt resurs styrning kan Azure Cosmos DB du tooelastically skala genomströmning över flera Azure-regioner på ett Azure DB som Cosmos-samling. Ändra dataflödet på en samling är en körningsåtgärd i Azure Cosmos-DB - som med andra databasåtgärder Azure Cosmos DB garanterar hello absolut övre gräns för svarstiden för din begäran toochange hello genomflöde. Exempelvis visar hello följande bild kundens samling med Elastiskt etablerat dataflöde (mellan 1-10M-begäranden per sekund mellan två regioner) baserat på hello begäran.
 
**En kund samling med Elastiskt etablerat dataflöde (1-10M begäranden per sekund)**

![Azure Cosmos-DB etablerat Elastiskt dataflöde](./media/distribute-data-globally/elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Genomströmnings relationen med konsekvenskontroll 
Samma som [Konsekvenskontrolls relation med genomströmning](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Genomströmnings relationen med tillgänglighet
Azure Cosmos-DB fortsätter toomaintain dess tillgänglighet när hello ändras toohello genomflöde. Azure Cosmos-DB transparent hanterar partitioner (exempelvis dela dokument, klona operations) och säkerställer att hello operations inte försämra prestanda eller tillgänglighet, medan programmet hello Elastiskt ökar eller minskar genomflöde. 

## <a id="AvailabilityGuarantees"></a>Garantier för tillgänglighet
Azure Cosmos-DB erbjuder en SLA med 99,99% drifttid tillgänglighet för varje hello data och kontroll plan åtgärder. Som beskrivits tidigare kan inkludera en absolut övre gräns för svarstid för alla data och kontroll plan som Azure Cosmos DB garantier för tillgänglighet. hello tillgänglighet garantier är steadfast och ändra inte med hello antal regioner eller geografiska avståndet mellan regioner. Garantier för tillgänglighet tillämpas med både manuell samt automatisk redundans. Azure Cosmos-DB erbjuder transparent flera API: er som se till att programmet kan köras mot logiska slutpunkter och transparent kan vidarebefordra hello begäranden toohello ny region vid redundans. Placera olika ditt program behöver inte toobe omdistribueras vid regional växling vid fel och hello tillgänglighets-SLA bevaras.

### <a id="AvailabilityAndConsistency"></a>Tillgänglighetsrelationen med konsekvenskontroll, svarstid och genomströmning
Tillgänglighetsrelationen med konsekvenskontroll, svarstid och genomströmning beskrivs i [Konsekvenskontrolls relationen med tillgänglighet](#ConsistencyAndAvailability), [Svarstids relationen med tillgänglighet](#LatencyAndAvailability) och [genomströmnings relationen med tillgänglighet](#ThroughputAndAvailability). 

## <a id="GuaranteesAgainstDataLoss"></a>Garanterar och systembeteendet för ”dataförlust”
I Azure Cosmos DB görs varje partition (för en samling) hög tillgänglighet med ett antal repliker som är fördelade på minst 10 20 feldomäner. Alla skrivningar arbetar synkront och varaktigt med ett kvorum majoritet av replikerna innan de bekräftas toohello klienten. Asynkron replikering används med samordning mellan partitioner som sprider sig över flera regioner. Azure Cosmos-DB garanterar att det finns ingen dataförlust för en klient-initierad manuell växling vid fel. Under automatisk redundans garanterar Azure Cosmos DB en övre gränsen för hello konfigurerats avgränsas föråldrad intervall i hello data går förlorade fönster som en del av sitt SERVICENIVÅAVTAL.

## <a id="CustomerFacingSLAMetrics"></a>Kund-riktade servicenivåavtal (SLA)
Azure Cosmos-DB exponerar transparent hello genomflöde, svarstid, konsekvens och tillgänglighet mått. De här måtten är tillgängliga och programmässigt via hello Azure-portalen (se följande figur). Du kan också ställa in aviseringar på olika tröskelvärden som använder Azure Application Insights.
 
**Konsekvenskontroll, svarstid, genomflöde och tillgänglighet är transparent tillgängliga tooeach klient**

![Azure DB Cosmos kunden synliga servicenivåavtal (SLA)](./media/distribute-data-globally/customer-slas.png)

## <a id="Next Steps"></a>Nästa steg
* tooimplement globala replikering på ditt Azure DB som Cosmos-kontot med hello Azure portal, se [hur tooperform Azure Cosmos DB globala databas replikering med hjälp av hello Azure-portalen](tutorial-global-distribution-documentdb.md).
* toolearn om hur tooimplement multimaster arkitekturer med Azure Cosmos DB finns [arkitekturer för flera master-databasen med Azure Cosmos DB](multi-region-writers.md).
* toolearn mer om hur automatisk och manuell redundans fungerar i Azure Cosmos DB finns [Regional växling vid fel i Azure Cosmos DB](regional-failover.md).

## <a id="References"></a>Referenser
1. Eric Brewer. [Mot Robust distribuerade system](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric Brewer. [CAP tolv år senare – hur hello regler har ändrats](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Bo Lynch. - [Brewer &#39; s antaganden och möjligheten att konsekvent, tillgänglig, Partition feltoleranta webbtjänster](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Mikael Abadi. [Konsekvenskontroll kompromisser i Modern distribuerade system databasstruktur](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. Ek Kleppmann. [Stoppa anropar databaser CP eller Asien](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis m.fl. [Probabilistic avgränsas föråldrad (PBS) för praktiska partiella beslutsförhet](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor och ull. [Läs in, kapacitet och tillgänglighet i kvorum system](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy och öljande. [Lineralizability: En är korrekt villkor för samtidiga objekt](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [SERVICENIVÅAVTAL för Azure Cosmos-DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
