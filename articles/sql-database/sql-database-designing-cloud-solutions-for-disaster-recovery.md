---
title: "aaaDesign tjänst med hög tillgänglighet med hjälp av Azure SQL Database | Microsoft Docs"
description: "Läs mer om programmet design för hög tillgänglighet med hjälp av Azure SQL Database-tjänster."
keywords: "molnet katastrofåterställning, katastrofåterställning, app-säkerhetskopiering, geo-replikering, kontinuerlig planering"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: e8a346ac-dd08-41e7-9685-46cebca04582
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 04/21/2017
ms.author: sashan
ms.openlocfilehash: 815f754ba7014cd8a1108a2d84c2a8f71d7030a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="designing-highly-available-services-using-azure-sql-database"></a>Designa tjänster med hög tillgänglighet med hjälp av Azure SQL Database

När du skapar och distribuerar tjänster med hög tillgänglighet på Azure SQL Database, använder du [redundans grupper och aktiv geo-replikering](sql-database-geo-replication-overview.md) tooprovide återhämtning tooregional fel och oåterkalleligt avbrott och aktivera snabb återställning toohello sekundära databaser. Den här artikeln fokuserar på vanliga program mönster och beskriver hello fördelar och avvägningarna med varje alternativ beroende på hello-krav för distribution av programmet, hello servicenivåavtalet du inriktar trafik svarstid och kostnader. Information om aktiv geo-replikering med elastiska pooler finns [elastisk Pool strategi för katastrofåterställning](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>Designmönstret 1: aktivt-passivt distribution för katastrofåterställning i molnet med en samplacerade-databas
Det här alternativet fungerar bäst för program med hello följande egenskaper:

* Aktiva instans i en enda Azure-region
* Stark beroendet av skrivskyddad (RW) åtkomst toodata
* Cross-region anslutningen mellan hello-webbprogram och hello databasen är inte giltig på grund av toolatency och trafik kostnad    

I det här fallet är hello topologi för distribution av program optimerad för hantering av regionala katastrofer när alla programkomponenter som påverkas och måste toofailover som en enhet. Hello programlogiken och hello databasen är replikerad tooanother region för geografisk redundans men används inte för hello programmet arbetsbelastning under normala hello. hello programmet hello sekundär region måste vara konfigurerade toouse en SQL-anslutning sträng toohello sekundär databas. Trafikhanterarprofilen har ställts in toouse [routningsmetoden för redundans](../traffic-manager/traffic-manager-configure-failover-routing-method.md).  

> [!NOTE]
> [Azure traffic manager](../traffic-manager/traffic-manager-overview.md) används endast för illustration i den här artikeln. Du kan använda belastningsutjämning lösning som har stöd för routningsmetoden för redundans.    
>

hello följande diagram visar denna konfiguration innan ett avbrott.

![Konfiguration av SQL Database geo-replikering. Katastrofåterställning i molnet.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Efter ett avbrott i hello primära region identifierar hello SQL Database-tjänsten hello primära databasen inte är tillgänglig och utlösa en växling vid fel toohello sekundär databas baserat på hello parametrar hello automatisk redundansprincip. Beroende på SERVICENIVÅAVTAL för programmet kan du bestämma tooconfigure en respitperiod mellan hello identifiering av hello avbrott och hello redundans sig själv. Konfigurera en respitperiod minskar risken för hello data går förlorade toohello fall där hello avbrott är allvarligt och tillgänglighet i hello region kan inte återställas snabbt. Om hello endpoint redundans initieras av hello traffic manager innan hello redundans grupp utlösare hello redundans för hello-databasen, hello webbprogram är inte kan tooreconnect toohello databas. hello programmet försök tooreconnect lyckas automatiskt så snart hello databasen redundansväxlingen är klar. 

> [!NOTE]
> tooachieve samordnad fullständigt redundans av programmet hello och hello databaser, bör du utforma din egen övervakningsmetod och använda manuell växling av hello web application slutpunkter och hello-databaser.
>

När hello redundans för både hello programmet slutpunkter och hello-databasen, hello programmet startas om bearbetning hello användarförfrågningar i hello region B och förblir samordnas med hello databasen eftersom hello primära databasen nu finns i region B. Det här scenariot illustreras i följande diagram hello. Alla diagram heldragen linje visar en aktiv anslutning, streckade linjer anger avbrutna anslutningar och stoppa loggar ange åtgärd utlösare.

![GEO-replikering: Redundans toosecondary databas. App-säkerhetskopiering.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Om ett avbrott inträffar i hello sekundär region hello replikeringslänk mellan hello primära och sekundära hello-databasen har pausats, men hello redundans aktiveras inte eftersom hello primära databasen inte påverkas. hello programmets tillgänglighet ändras inte i det här fallet men hello programmet fungerar exponerade och därför högre risk om båda regioner misslyckas i följd.

> [!NOTE]
> Vi rekommenderar hello konfiguration med programmet distribution begränsad tootwo regioner för katastrofåterställning. Detta beror på att de flesta av hello Azure geografiska områden har bara två områden. Den här konfigurationen skyddar inte programmet från ett samtidiga oåterkalleligt fel på båda regioner.  En osannolika av ett sådant fel kan du återställa databaserna i en tredje region med [geo-återställning åtgärden](sql-database-disaster-recovery.md#recover-using-geo-restore).
>

När hello avbrott minskas synkroniseras automatiskt hello sekundära databasen igen med primär hello. Under synkroniseringen gick av hello primära något påverkas databasprestanda beroende på hello mängden data som behöver toobe synkroniseras. hello illustrerar följande diagram ett avbrott i hello sekundär region.

![Sekundär databas synkroniseras med primär. Katastrofåterställning i molnet.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)

hello nyckeln **fördelar** av det här designmönstret är:

* hello är samma webbprogram distribuerade tooboth regioner utan någon regionspecifika konfiguration och inga ytterligare logik tooreact toohello redundans. 
* hello programmets prestanda påverkas inte av redundans som hello-webbprogram och hello databas är alltid samordnas.

hello huvudsakliga **förhållandet** är att hello redundant programmet instans i hello sekundär region används endast för katastrofåterställning.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>Designmönstret 2: aktiv-aktiv distribution för belastningsutjämning för program
Det här alternativet för molnet disaster recovery fungerar bäst för program med hello följande egenskaper:

* Hög förhållandet mellan databasen läser toowrites
* Databasen läsa svarstiden är mer kritiska för hello slutanvändarens upplevelse än hello skrivfördröjningen 
* Skrivskyddad logik kan skiljas från skrivskyddad logik med hjälp av en annan anslutningssträng
* Skrivskyddad logik är inte beroende data synkroniseras fullständigt med hello senaste uppdateringarna  

Om dina program har följande egenskaper, belastningsutjämning hello slutanvändarens anslutningar mellan flera programinstanser i olika regioner kan avsevärt förbättra hello övergripande slutanvändarens upplevelse. Två av hello regioner är markerat som hello DR-par och hello redundans gruppen bör innehålla hello databaser i dessa regioner. tooimplement belastningsutjämning, bör varje region har en aktiv instans av programmet hello med hello skrivskyddad (RW) logik anslutna toohello skrivskyddad lyssnare slutpunkt hello redundans gruppen. Det garanterar att hello redundansväxlingen automatiskt initieras om hello primära databasen påverkas av ett avbrott. hello skrivskyddad logiken (RO) i hello webbprogram ska ansluta direkt toohello databasen i den regionen. Traffic manager ska ställas in toouse [prestanda routning](../traffic-manager/traffic-manager-configure-performance-routing-method.md) med [slutpunkt övervakning](../traffic-manager/traffic-manager-monitoring.md) aktiverat för varje förekomst av programmet.

Som i mönstret #1, bör du distribuera en liknande övervakningsprogram. Men till skillnad från mönstret #1 hello övervakning av program kan inte ansvarar för att utlösa hello slutpunkt redundans.

> [!NOTE]
> När det här mönstret använder mer än en sekundär databas, endast hello sekundär i Region B ska användas för redundans och ska vara en del av hello redundansväxlingsgrupp.
>

Traffic manager ska konfigureras för prestanda routning toodirect hello användare anslutningar toohello programmet instans närmaste toohello användarens geografisk plats. hello följande diagram illustrerar den här konfigurationen innan ett avbrott.

![Inga avbrott: prestanda routning toonearest program. GEO-replikering.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Om ett avbrott i databasen har upptäckts i hello region A hello redundans gruppen automatiskt att starta redundans för hello primära databasen i region A toohello sekundär region B. Hello skrivskyddad lyssnare endpoint tooregion B uppdateras automatiskt så att läsa / skriva-anslutningar i hello webbprogram inte påverkas. hello traffic manager utesluta hello offline slutpunkt från hello routningstabellen men fortsätter routning hello slutanvändarens trafik toohello återstående instanser online. hello skrivskyddad SQL-anslutningssträngar påverkas inte samma som de alltid toohello databasen i hello samma region. 

hello följande diagram illustrerar hello ny konfiguration efter hello växling vid fel.

![Konfiguration efter växling vid fel. Katastrofåterställning i molnet.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

Vid ett avbrott i någon av hello sekundära regioner tar hello traffic manager automatiskt bort hello offline slutpunkt i regionen från hello-routningstabell. hello replikering kanal toohello sekundär databas i den regionen kommer att inaktiveras. Eftersom hello återstående regioner få ytterligare användartrafiken i det här scenariot, påverkas hello programmets prestanda under hello avbrott. När hello avbrott minskas synkroniseras hello sekundär databas i hello påverkas region omedelbart med hello primära. Under hello gick synkroniseringsprestanda för hello primära något påverkas beroende på hello mängden data som behöver toobe synkroniseras. hello följande diagram illustrerar ett avbrott i region B.

![Avbrott i sekundär region. Katastrofåterställning i molnet - geo-replikering.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

hello nyckeln **nytta** för den här designen mönstret är att du kan skala hello programmet arbetsbelastningen över flera sekundärservrar tooachieve hello slutanvändarens optimala prestanda. Hej **kompromisser** detta alternativ är:

* Skrivskyddad anslutningar mellan programinstanser hello och databasen har olika svarstid och kostnad
* Programmets prestanda påverkas under hello avbrott

> [!NOTE]
> Ett liknande sätt kan användas för toooffload specialiserade arbetsbelastningar som rapportering jobb, verktyg för business intelligence eller säkerhetskopior. Normalt dessa arbetsbelastningar använder betydande databasresurser därför rekommenderas det att du anger en hello sekundära databaser för dem med hello prestanda nivån matchade toohello förväntade arbetsbelastning.
>

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>Designmönstret 3: aktivt-passivt distribution för förvaring av data
Det här alternativet fungerar bäst för program med hello följande egenskaper:

* Dataförlust är viktiga för verksamheten risk. hello databasfel kan endast användas som en sista utväg om hello avbrott är oåterkalleligt.
* hello-programmet stöder skrivskyddade och läsa / skriva-lägen för åtgärder och kan köras ”skrivskyddad” för en viss tidsperiod.

I det här mönstret växlar programmet hello läget endast tooread när hello skrivskyddad anslutningar börja få timeout-fel. hello webbprogram är distribuerade tooboth regioner och innehåller en anslutning toohello skrivskyddad lyssnare slutpunkt och en annan anslutning toohello skrivskyddad lyssnare slutpunkt. hello Traffic manager ska ställas in toouse [redundans routning](../traffic-manager/traffic-manager-configure-failover-routing-method.md) med [slutpunkt övervakning](../traffic-manager/traffic-manager-monitoring.md) aktiverad för hello programslutpunkt i varje region.

hello följande diagram illustrerar den här konfigurationen innan ett avbrott.

![Aktivt-passivt distribution före redundans. Katastrofåterställning i molnet.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

När hello traffic manager upptäcker en anslutning fel tooregion A, växlar automatiskt trafiken toohello programmet användarinstans i region B. Med det här mönstret är det viktigt att du ställer in hello respitperioden med förlust tooa tillräckligt högt värde, t.ex. 24 timmar. Det garanterar att data går förlorade förhindras om hello avbrott minskas inom den tiden. När hello webbprogram i hello region B är aktiverad startar hello Läs-och skrivåtgärder misslyckas. Då ska den växla toohello skrivskyddat läge. I det här läget hello begäranden inte automatiskt routade toohello sekundär databas. Hello gäller ett katastrofalt fel hello avbrott inte ska undvikas inom respitperioden hello och hello redundansväxlingsgrupp utlöser hello växling vid fel. Efter att hello skrivskyddad lyssnare blir tillgängliga och hello anrop tooit stoppas misslyckas. Detta visas hello följande diagram.

![Avbrott: Programmet i skrivskyddat läge. Katastrofåterställning i molnet.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Om hello avbrott i hello primära region minskas inom respitperioden hello traffic manager identifierar hello återställning av anslutning i hello primär region och växlar trafiken tillbaka toohello programmet användarinstans i region A. Den programinstansen återgår och körs i läsa / skriva-läge med hjälp av hello primära databasen i region A.

Vid ett strömavbrott i hello region B identifierar hello traffic manager hello fel i hello programmet slutpunkt i region B och hello redundans gruppen växlar hello skrivskyddad lyssnare tooregion A. Den här avbrott påverkar inte hello slutanvändarens upplevelse men hello primära databasen kommer att exponeras under hello avbrott. Detta visas hello följande diagram.

![Avbrott: Sekundär databas. Katastrofåterställning i molnet.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

När hello avbrott minskas hello sekundära databasen synkroniseras omedelbart med hello primära och hello skrivskyddad lyssnare är avstängda tillbaka toohello sekundär databas i region B. Under synkroniseringen gick av hello primära något påverkas databasprestanda beroende på hello mängden data som behöver toobe synkroniseras.

Det här designmönstret har flera **fördelar**:

* Den förhindrar dataförlust under hello tillfälliga avbrott.
* Driftstopp beror på hur snabbt traffic manager identifierar hello anslutningsfel, som kan konfigureras.

Hej **förhållandet** är:

* Programmet måste vara kan toooperate i skrivskyddat läge.

> [!NOTE]
> Vid en permanent avbrott i hello region du manuellt Aktivera databasen redundans och accepterar hello dataförlust. hello programmet kommer att fungera i hello sekundär region med läs-/ skrivåtkomst toohello databas.
>

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Kontinuitet planering: Välj en design för programmet för katastrofåterställning i molnet
Din strategi för katastrofåterställning specifika molnet kan kombinera eller utöka dessa designmönster toobest hello uppfyller behoven för ditt program.  Som tidigare nämnts är hello strategi som du väljer baserad på hello SLA och du vill toooffer tooyour kunder hello topologi för distribution av programmet. toohelp hjälper ditt beslut, hello följande tabell jämförs hello alternativ baserat på hello uppskattade data förlust eller återställning mål för återställningspunkt (RPO) och uppskattade återställningstid (Infoga).

| Mönstret | ÅTERSTÄLLNINGSPUNKTMÅL | INFOGA |
|:--- |:--- |:--- |
| Aktivt-passivt distribution för katastrofåterställning med samordnade databasåtkomst |Läs-/ skrivåtkomst < 5 SEK. |Fel identifieringstiden + DNS TTL |
| Aktiv-aktiv distribution för belastningsutjämning för program |Läs-/ skrivåtkomst < 5 SEK. |Fel identifieringstiden + DNS TTL |
| Aktivt-passivt distribution för förvaring av data |Skrivskyddad åtkomst < 5 SEK. | Skrivskyddad åtkomst = 0 |
||Läs-/ skrivåtkomst = noll | Läs-/ skrivåtkomst = fel identifieringstiden + respitperioden med dataförlust |
|||

## <a name="next-steps"></a>Nästa steg
* toolearn om Azure SQL Database automatisk säkerhetskopiering finns [SQL-databas automatisk säkerhetskopiering](sql-database-automated-backups.md)
* En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md)
* toolearn om hur du använder automatisk säkerhetskopiering för återställning finns [återställa en databas från hello service-initierad säkerhetskopiering](sql-database-recovery-using-backups.md)
* toolearn om snabbare återställningsalternativ finns [aktiv geo-replikering](sql-database-geo-replication-overview.md)  
* toolearn om hur du använder automatisk säkerhetskopiering för arkivering, se [databaskopieringen](sql-database-copy.md)
* Information om aktiv geo-replikering med elastiska pooler finns [elastisk Pool strategi för katastrofåterställning](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
