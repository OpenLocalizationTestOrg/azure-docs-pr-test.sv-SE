---
title: "aaaDesign lösningar för katastrofåterställning - Azure SQL Database | Microsoft Docs"
description: "Lär dig hur toodesign din molnlösning för katastrofåterställning genom att välja hello rätt redundans mönster."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2db99057-0c79-4fb0-a7f1-d1c057ec787f
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: sashan;carlrab
ms.openlocfilehash: 9d9eca7570c7a01c992d0b33d449721709b471c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>Strategi för katastrofåterställning för program som använder SQL-databas elastiska pooler
Vi har lärt dig att molntjänster inte felsäker och oåterkalleligt incidenter inträffa hello års. SQL-databasen innehåller flera funktioner tooprovide för hello Företagskontinuitet för programmet när dessa händelser inträffar. [Elastiska pooler](sql-database-elastic-pool.md) och enskilda databaser stöd hello samma typ av funktioner för katastrofåterställning. Den här artikeln beskriver flera DR strategier för elastiska pooler som utnyttjar funktionerna för verksamhetskontinuitet dessa SQL-databas.

Den här artikeln använder hello följande kanoniska SaaS ISV-programmönster:

<i>En modern molnbaserad webbprogrammet etablerar en SQL-databas för varje slutanvändare. hello ISV har många kunder och därför använder många databaser, kallas även klient databaser. Eftersom hello klient databaser har vanligtvis oförutsägbart aktivitet mönster, använder hello ISV en elastisk pool toomake hello databaskostnader mycket förutsägbar under en längre tid. hello elastisk pool förenklar också hello prestandahantering när hello användaraktivitet ger spikar i diagrammet. Dessutom toohello klient databaser hello program också använder flera databaser toomanage användarprofiler, säkerhet, samla in användningsmönster osv. Tillgängligheten för hello enskilda klienter påverkar inte hello programmets tillgänglighet som helhet. Dock hello tillgänglighet och prestanda för hantering av databaser är viktig för funktionen hello programmet och om hello management databaser är offline hello hela programmet är offline.</i>  

Den här artikeln beskrivs DR strategier som omfattar ett antal scenarier från kostnaden känsliga Start program tooones med strikta tillgänglighet.

## <a name="scenario-1-cost-sensitive-startup"></a>Scenario 1. Kostnad känsliga Start
<i>Jag är en start-företag och är mycket kostar känslig.  Jag vill toosimplify distribution och hantering av programmet hello och det kan ha begränsad SLA för enskilda kunder. Men jag vill tooensure hello programmet som helhet aldrig är offline.</i>

toosatisfy hello enkelhet kravet distribuera alla klient-databaser i en elastisk pool i hello Azure-region önskat och distribuera management databaser som georeplikerad enskilda databaser. Använd geo-återställning som levereras utan extra kostnad för hello katastrofåterställning av innehavare. tooensure hello tillgängligheten för hello management databaser, geo-replikering dem tooanother region med hjälp av en grupp med automatisk växling vid fel (i förhandsvisning) (steg 1). hello är pågående hello katastrofberedskapskonfigurationen i det här scenariot lika toohello totalkostnaden för hello sekundära databaser. Den här konfigurationen visas på nästa hello-diagram.

![Bild 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Om ett avbrott inträffar i hello primära region, hello recovery steg toobring tillämpningsprogrammet online illustreras med hello nästa diagram.

* Hej redundansväxlingsgrupp initierar automatisk redundans av hello management databasen toohello DR region. hello program är automatiskt återanslutna toohello nya primära och alla nya konton och klient-databaser skapas i hello DR region. hello befintliga kunder finns data är för tillfället otillgänglig.
* Skapa hello elastisk pool med hello samma konfiguration som hello ursprungliga poolen (2).
* Använd geo-återställning toocreate kopior av hello klient databaser (3). Du kan överväga utlösa hello enskilda återställningar av hello slutanvändarens anslutningar eller använda vissa andra programspecifika prioritet schema.


Nu ditt program är tillbaka online i hello DR region, men vissa kunder uppstår fördröjning vid åtkomst till sina data.

![Bild 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Om hello avbrott var temporära, är det möjligt den primära regionen hello återställs med Azure innan alla hello databasen återställningar har slutförts i hello DR region. I det här fallet samordna glidande hello programmet tillbaka toohello primär region. hello processen tar hello steg som visas i nästa hello-diagrammet.

* Avbryt alla väntande geo-återställning begäranden.   
* Växla över hello management databaser toohello primär region (5). Hello gamla primärfärgerna har automatiskt blivit sekundärservrar efter hello region återställningen. De växla roller igen. 
* Ändra hello programmet anslutning sträng toopoint tillbaka toohello primär region. Nu skapas alla nya konton och klient-databaser i hello primär region. Vissa befintliga kunder finns data är för tillfället otillgänglig.   
* Ange alla databaser i hello DR poolen endast tooread tooensure, inte kan ändras i hello DR region (6). 
* Byt namn på eller ta bort motsvarande hello-databaser i hello primära pool (7) för varje databas i hello DR-pool som har ändrats sedan hello återställning. 
* Kopiera hello uppdatera databaser från hello DR poolen toohello primära pool (8). 
* Ta bort hello DR-pool (9)

Tillämpningsprogrammet är nu online i hello primära region med alla klient-databaser finns i hello primära pool.

![Bild 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

hello nyckeln **dra** för den här strategin är låg pågående kostnad för dataredundans nivå. Säkerhetskopieringar tas automatiskt av hello SQL Database-tjänsten med inga program omarbetning och utan extra kostnad.  hello kostnad uppkommer endast när hello elastiska databaser återställs. Hej **kompromiss** är att hello slutföra återställningen av alla klient-databaser tar betydande tid. hello tid beror på hello totala antalet återställningar som du startar i hello DR region och storleken på hello klient databaser. Även om du prioritera vissa klienter återställningar framför andra konkurrerar med alla hello hello andra återställningar som startas i samma region som hello tjänsten hanterar och begränsar toominimize hello övergripande effekten på hello befintliga kunders databaser. Dessutom kan inte hello återställning av databaser för hello-klient starta förrän hello ny elastisk pool i hello DR region har skapats.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Scenario 2. Mogen program med nivåindelade service
<i>Jag är en mogen SaaS-program med nivåindelade tjänsten ger och annan serviceavtal för utvärderingsversion kunder och för att betala kunder. Hello utvärderingsversion kunder har jag tooreduce hello kostnad så mycket som möjligt. Utvärderingsversion kunder kan ta avbrottstid men jag vill tooreduce dess sannolikheten. Hello betalar kunder, är driftavbrott svarta risk. Jag vill toomake till att betalande kunder är alltid kan tooaccess sina data.</i> 

Det här scenariot separat hello utvärderingsversioner för klienter från toosupport betald klienter genom att placera dem i separata elastiska pooler. hello utvärderingsversion kunder har lägre eDTU per klient och lägre SLA med en längre återställningstid. hello betalande kunder finns i en pool med högre eDTU per klient och en högre SLA. tooguarantee hello lägsta återställningstid hello betalande kunders klient databaser är georeplikerad. Den här konfigurationen visas på nästa hello-diagram. 

![Bild 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Hello management databaser är ganska active som hello första scenariot, så att du använder en enda georeplikerad databas för det (1). Detta säkerställer hello förutsägbar prestanda för nya kundprenumerationer, profiluppdateringar och andra hanteringsåtgärder. hello region där hello primärfärgerna hello management databaser finns är hello primär region och hello region där hello sekundärservrar hello management databaser finns är hello DR region.

hello har betalande kunder klient databaser aktiva databaser i hello ”betald” pool som har etablerats i hello primära region. Etablera en sekundär pool med samma namn i hello DR region hello. Varje klient är georeplikerad toohello sekundära pool (2). Detta gör att snabb återställning av alla klient-databaser med växling vid fel. 

Om ett avbrott inträffar i hello primära region, hello recovery steg toobring tillämpningsprogrammet online visas i nästa hello-diagram:

![Bild 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Omedelbart växla över hello management databaser toohello DR region (3).
* Ändra hello programmet anslutning sträng toopoint toohello DR region. Alla nya konton och klient databaser skapas nu i hello DR region. hello befintliga utvärderingsversion kunder finns data är för tillfället otillgänglig.
* Redundans hello betald klients databaser toohello pool i hello DR region tooimmediately återställa deras tillgänglighet (4). Eftersom hello redundans är en snabb nivån metadataändring, överväga en optimering där hello enskilda redundans initieras på begäran av hello slutanvändarens anslutningar. 
* Om storleken på din sekundära pool-eDTU är lägre än hello primära eftersom hello sekundära databaser endast obligatoriska hello kapacitet tooprocess hello ändra loggar när de har sekundärservrar, öka omedelbart hello pool-kapacitet nu tooaccommodate hello fullständig arbetsbelastningen för alla klienter (5). 
* Skapa hello ny elastisk pool med hello samma namn och hello samma konfiguration i hello DR region för hello utvärderingsversion kunders databaser (6). 
* När hello utvärderingsversion kunders adresspoolen har skapats kan du använda geo-återställning toorestore hello utvärderingsinnehavare för enskilda databaser i hello ny pool (7). Överväg att utlösa hello enskilda återställningar av hello slutanvändarens anslutningar eller använda vissa andra programspecifika prioritet schema.

Tillämpningsprogrammet är nu online i hello DR region. Alla betalande kunder har åtkomst tootheir data medan hello utvärderingsversion kunder uppstår fördröjning vid åtkomst till sina data.

När hello primär region återställs med Azure *när* du har återställt hello programmet hello DR-region som du kan fortsätta att köra programmet hello i regionen eller så kan du bestämma toofail tillbaka toohello primär region. Om hello primär region återställs *innan* hello failover-processen är klar bör du överväga att misslyckas tillbaka direkt. återställning av hello tar hello steg som visas i nästa hello-diagram: 

![Bild 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Avbryt alla väntande geo-återställning begäranden.   
* Växla över hello management databaser (8). Efter återställning av hello region hello gamla primära blir automatiskt hello sekundär. Nu blir hello primära igen.  
* Växla över hello betald klient databaser (9). På liknande sätt efter återställning av hello region blir hello gamla primärfärgerna automatiskt hello sekundärservrar. Nu blir de hello primärfärgerna igen. 
* Ange hello återställt utvärderingsversion databaser som har ändrats i hello DR region endast tooread (10).
* För varje databas i hello utvärderingsversion kunder DR pool som ändrats sedan hello recovery Byt namn eller ta bort motsvarande hello-databasen i hello utvärderingsversion kunder primära pool (11). 
* Kopiera hello uppdatera databaser från hello DR poolen toohello primära pool (12). 
* Ta bort hello DR-pool (13) 

> [!NOTE]
> hello Redundansåtgärden är asynkron. toominimize hello återställningstid är det viktigt att du kör hello klient databaser redundanskommandot i grupper med minst 20 databaser. 
> 
> 

hello nyckeln **nytta** av den här strategin är att det ger högsta hello-SLA för hello betalar kunder. Det garanterar att hello nya försök är blockerad när hello DR testpoolen skapas. Hej **kompromiss** är att den här installationen ökar hello totalkostnaden för hello klient databaser av hello kostnaden för hello sekundära DR-adresspool för betald kunder. Dessutom om hello sekundära poolen har olika storlekar, prestanda hello betalande kunder lägre efter redundans tills hello poolen i hello DR region uppgraderingen. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Scenario 3. Geografiskt distribuerade program med nivåindelade service
<i>Jag har en mogen SaaS-program med nivåindelade service erbjudanden. Jag vill toooffer en mycket aggressiv SLA toomy betald kunder och minimera hello risken för påverkan när avbrott inträffar eftersom även kort avbrott kan orsaka kunden klagomål. Det är viktigt att hello betalar kunder alltid kan komma åt sina data. hello-utvärderingar är gratis och ett SLA erbjuds inte under hello utvärderingsperioden.</i> 

toosupport det här scenariot, Använd tre separata elastiska pooler. Etablera två lika storlek pooler med hög edtu: er per databas i två olika regioner toocontain hello betald kunders klient databaser. hello tredje pool som innehåller hello utvärderingsversioner för klienter kan ha lägre edtu: er per databas och etableras i någon av hello två regioner.

tooguarantee hello lägsta återställningstid under avbrott hello betalande kunders klient databaser är georeplikerad med 50% av primära hello-databaser i varje hello två regioner. Dessutom har varje region 50% av hello sekundära databaser. På så sätt kan om en region är offline, högst 50% av hello betald kunders databaser påverkas och har toofail över. hello påverkas andra databaser inte. Den här konfigurationen illustreras i följande diagram hello:

![Bild 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Som i föregående fall hello hello management databaser är ganska aktiva så konfigurera dem som enskilda geo-replikerade databaser (1). Detta säkerställer hello förutsägbar prestanda hello nya kundprenumerationer, profiluppdateringar och andra hanteringsåtgärder. Region A är hello primära region för hello management databaser och hello region B används för återställning av hello management databaser.

hello betalande kunders klient databaser är också georeplikerad men med primärfärgerna och sekundärservrar delas upp mellan A och B (2)-region. Det här sättet hello klient primära databaser påverkas av hello kan växla över toohello andra region och blir tillgängliga. hello andra hälften av hello klient databaser inte är påverkas alls. 

hello nästa diagram illustrerar hello recovery steg tootake om ett avbrott inträffar i region A.

![Bild 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* Omedelbart växla över hello management databaser tooregion B (3).
* Ändra hello programmet anslutning sträng toopoint toohello management databaser i region B. Ändra hello management databaser toomake hello nya konton och klient databaser skapas i region B och hello befintlig klient databaser finns det som bra. hello befintliga utvärderingsversion kunder finns data är för tillfället otillgänglig.
* Växla över hello betald klient databaser toopool 2 i region B tooimmediately Återställ deras tillgänglighet (4). Eftersom hello redundans är en snabb nivån metadataändring, kan du en optimering där hello enskilda redundans initieras på begäran av hello slutanvändarens anslutningar. 
* Sedan nu poolen 2 innehåller endast primära databaser, hello totala arbetsbelastningen i hello poolen ökar och omedelbart öka storleken eDTU (5). 
* Skapa hello ny elastisk pool med hello samma namn och hello samma konfiguration i hello region B i hello utvärderingsversion kunders databaser (6). 
* När hello poolen har skapats använda geo-återställning toorestore hello enskilda utvärderingsinnehavare databas i hello pool (7). Du kan överväga utlösa hello enskilda återställningar av hello slutanvändarens anslutningar eller använda vissa andra programspecifika prioritet schema.

> [!NOTE]
> hello Redundansåtgärden är asynkron. toominimize hello återställningstid är det viktigt att du kör hello klient databaser redundanskommandot i grupper med minst 20 databaser. 
> 

Nu är ditt program tillbaka online i region B. Alla betalande kunder har åtkomst tootheir data medan hello utvärderingsversion kunder uppstår fördröjning vid åtkomst till sina data.

När region A återställs måste toodecide om du vill toouse region B för utvärderingsversion kunder eller återställning efter fel toousing hello utvärderingsversion kunder poolen i region A. Ett villkor kan vara hello % utvärderingsinnehavare databaser har ändrats sedan hello återställning. Du måste toore saldo hello betald hyresgäster mellan två pooler oavsett detta beslut. hello nästa diagram illustrerar processen för hello när hello utvärderingsinnehavare databaser misslyckas tillbaka tooregion A.  

![Bild 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Avbryt alla väntande geo-återställning begäranden tootrial DR-pool.   
* Växla över hello-databasen för konfigurationshantering (8). Efter återställning av hello region blev hello gamla primära automatiskt hello sekundär. Nu blir hello primära igen.  
* Välj vilka klient betalda databaser misslyckas tillbaka toopool 1 och initiera redundans tootheir sekundärservrar (9). Efter återställning av hello region måste blev alla databaser i poolen 1 automatiskt sekundärservrar. Nu blir 50% av dem primärfärgerna igen. 
* Minska hello storleken på poolen 2 toohello ursprungliga eDTU (10).
* Ange alla återställt utvärderingsversion databaser i hello region B endast tooread (11).
* För varje databas i hello DR testpoolen som har ändrats sedan hello recovery Byt namn eller ta bort motsvarande hello-databasen i hello primära testpoolen (12). 
* Kopiera hello uppdatera databaser från hello DR poolen toohello primära pool (13). 
* Ta bort hello DR-pool (14) 

hello nyckeln **fördelar** av den här strategin är:

* Det stöder hello mest SLA för hello betalar kunder eftersom det innebär att avbrott inte påverkar mer än 50% av hello klient databaser. 
* Det garanterar att hello nya försök är blockerad när hello spår DR-poolen har skapats under hello återställningen. 
* Den innebär en effektivare användning av hello pool-kapacitet som 50% av sekundära databaser i pool 1 och pool 2 garanteras toobe mindre aktiva hello primära databaser.

hello huvudsakliga **avvägningarna** är:

* Hej CRUD-åtgärder mot hello management databaser har kortare svarstid för hello slutanvändare anslutna tooregion A än för hello slutanvändare anslutna tooregion B som de körs mot hello primära hello management databaser.
* Det krävs mer komplexa utformningen av hello management databas. Varje klient-post har till exempel en platstagg som behöver toobe ändras under redundans och återställning efter fel.  
* hello betalar kunder kan prestanda lägre än vanligt tills hello poolen uppgraderingen i region B är klar. 

## <a name="summary"></a>Sammanfattning
Den här artikeln fokuserar på hello strategi för katastrofåterställning för hello databasnivå som används av ett SaaS ISV-program för flera innehavare. hello strategi som du väljer är baserad på hello behov hello program, till exempel hello affärsmodell, hello SLA du vill toooffer tooyour kunder, budget begränsningen osv. Alla beskrivs strategi beskrivs hello fördelar och kompromiss så att du kan fatta ett välgrundat beslut. Dessutom innehåller ett visst program som sannolikt andra Azure-komponenter. Så att du kan granska deras business continuity vägledning och samordna hello återställning av hello databasnivå med dem. toolearn mer information om hur du hanterar återställning av databasprogram i Azure, referera för[molnlösningar för utformning för katastrofåterställning](sql-database-designing-cloud-solutions-for-disaster-recovery.md).  

## <a name="next-steps"></a>Nästa steg
* toolearn om Azure SQL Database automatisk säkerhetskopiering finns [SQL-databas automatisk säkerhetskopiering](sql-database-automated-backups.md).
* En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).
* toolearn om hur du använder automatisk säkerhetskopiering för återställning, se [återställa en databas från hello service-initierad säkerhetskopiering](sql-database-recovery-using-backups.md).
* toolearn om snabbare återställningsalternativ finns [aktiv geo-replikering](sql-database-geo-replication-overview.md).
* toolearn om hur du använder automatisk säkerhetskopiering för arkivering, se [databaskopieringen](sql-database-copy.md).

