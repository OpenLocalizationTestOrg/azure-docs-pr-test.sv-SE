---
title: aaaRolling programuppgraderingar - Azure SQL Database | Microsoft Docs
description: "Lär dig hur toouse Azure SQL Database geo-replikering toosupport online uppgraderingar av ditt program i molnet."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 58f42859-1e37-463c-a3d8-a3ca2e867148
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/16/2016
ms.author: sashan
ms.openlocfilehash: 18c56300916d129bff141624cc5c416b500408d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Hantera löpande uppgraderingar av molnprogram som använder SQL-databas aktiv geo-replikering
> [!NOTE]
> [Aktiv geo-replikering](sql-database-geo-replication-overview.md) är nu tillgänglig för alla databaser i alla nivåer.
> 

Lär dig hur toouse [georeplikering](sql-database-geo-replication-overview.md) i SQL-databasen tooenable löpande uppgradering av ditt program i molnet. Uppgraderingen är en störande åtgärd, bör den vara en del av ditt företag kontinuitet planerings- och. I den här artikeln som vi titta på två olika metoder för att samordna hello uppgraderingsprocessen och diskutera hello fördelar och avvägningarna med varje alternativ. Hello enligt den här artikeln ska vi använda ett enkelt program som består av en webbplats anslutna tooa databas som sin datanivå. Vårt mål är tooupgrade version 1 av hello programmet tooversion 2 utan betydande påverkan på hello slutanvändarens upplevelse. 

När du utvärderar hello uppgraderingsalternativ bör du hello följande faktorer:

* Inverkan på tillgänglighet under uppgraderingar. Hur länge hello programmet funktionen begränsad eller vara försämrad.
* Möjlighet tooroll tillbaka om ett fel uppstår på uppgradera.
* Säkerhetsproblem för hello program om en orelaterade katastrofalt fel inträffar under hello uppgradering.
* Totalt antal dollar kostnad.  Detta omfattar ytterligare redundans och kostnader för hello tillfälliga komponenter som används av hello uppgraderingsprocessen. 

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Uppgradera program som förlitar sig på säkerhetskopiering av databaser för katastrofåterställning
Om ditt program använder automatiska säkerhetskopieringar och använder geo-återställning för katastrofåterställning, är det vanligtvis distribuerade tooa enda Azure-region. I det här fallet innebär hello uppgraderingen att du skapar en säkerhetskopiering distribution av alla programkomponenter ingår i hello uppgraderingen. toominimize hello slutanvändaren du använder Azure Traffic Manager (WATM) med hello redundans profil.  hello följande diagram illustrerar hello driftsmiljön tidigare toohello uppgraderingsprocessen. Hej endpoint <i>contoso 1.azurewebsites.net</i> representerar en produktionsplatsen för hello-program som behöver toobe uppgraderas. tooenable hello möjlighet tooroll tillbaka Hej uppgraderingen, du måste skapa en steg-plats med en helt synkroniserad kopia av programmet hello. hello är följande steg nödvändiga tooprepare hello programmet hello uppgraderingen:

1. Skapa en plats för fas för hello uppgraderingen. toodo som skapar en sekundär databas (1) och distribuera en identisk webbplats i hello samma Azure-region. Övervaka hello sekundära toosee om hello seeding processen har slutförts.
2. Skapa en profil för växling vid fel i WATM med <i>contoso 1.azurewebsites.net</i> som online slutpunkt och <i>contoso 2.azurewebsites.net</i> som offline. 

> [!NOTE]
> Obs hello förberedelsesteg påverkar inte programmet hello i hello produktionsplatsen och den kan fungera i full åtkomst-läge.
>  

![Konfiguration av SQL-databasen gå-replikeringen. Katastrofåterställning i molnet.](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

När hello förberedelsesteg har slutförts är hello programmet redo för hello verkliga uppgraderingen. hello följande diagram illustrerar hello steg som ingår i hello uppgraderingsprocessen. 

1. Ange hello primära databasen i hello fack endast tooread Produktionsläge (3). Detta garanterar att hello produktion instans av programmet hello (V1) förblir skrivskyddad uppgraderar hello vilket gör hello dataavvikelser mellan hello V1 och V2-databasinstanser.  
2. Koppla från hello sekundär databas med hjälp av hello planerad avslutning läge (4). Den skapar en helt synkroniserad oberoende kopia av hello primära databasen. Den här databasen kommer att uppgraderas.
3. Aktivera hello primära databasen tooread-och skrivbehörighet och kör hello uppgraderingsskriptet hello steget fack (5).     

![Konfiguration av SQL Database geo-replikering. Katastrofåterställning i molnet.](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Om hello uppgraderingen har slutförts men du är nu redo tooswitch hello slutanvändare toohello mellanlagrad kopiera hello program. Det blir nu hello produktionsplatsen av programmet hello.  Detta omfattar några fler steg som visas på hello följande diagram.

1. Växla hello online slutpunkt i hello WATM profil för<i>contoso 2.azurewebsites.net</i>, vilken punkter toohello V2 version av hello-webbplats (6). Nu blir hello produktionsplatsen med hello V2 program och hello slutanvändarens trafiken dirigerad tooit.  
2. Om du inte längre behöver hello V1 programkomponenter så du kan ta bort dem (7).   

![Konfiguration av SQL Database geo-replikering. Katastrofåterställning i molnet.](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Om hello uppgraderingsprocessen misslyckas, till exempel på grund av tooan fel i hello uppgraderingsskript, hello steget fack bör övervägas komprometteras. tooroll tillbaka hello toohello förberedande programtillstånd du bara återställa hello programmet hello produktion fack toofull åtkomst. hello stegen visas i nästa hello-diagrammet.    

1. Ange hello databasen kopiera tooread-och skrivbehörighet (8). Detta återställer hello fullständig V1 funktionellt hello produktionsplatsen.
2. Utför hello Rotorsaksanalys och ta bort hello komprometteras komponenter i hello steget fack (9). 

Nu hello programmet fungerar fullt ut och hello Uppgraderingsstegen kan upprepas.

> [!NOTE]
> hello rollback inte kräver ändringar i WATM profilen eftersom den redan pekar för<i>contoso 1.azurewebsites.net</i> som hello aktiv slutpunkt.
> 
> 

![Konfiguration av SQL Database geo-replikering. Katastrofåterställning i molnet.](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

hello nyckeln **nytta** för det här alternativet är att du kan uppgradera ett program i en region med en uppsättning enkla steg. hello är dollar hello uppgraderingen relativt låg. hello huvudsakliga **förhållandet** är som om ett oåterkalleligt fel uppstår under hello uppgradera hello toohello förberedande återställningstillstånd kommer att omfatta ny distribution av programmet hello i en annan region och återställa hello-databas från säkerhetskopiering med geo-återställning. Den här processen medför betydande driftstopp.   

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Uppgradera program som förlitar sig på database geo-replikering för katastrofåterställning
Om ditt program utnyttjar geo-replikering för verksamhetskontinuitet, är det distribuerade tooat minst två olika regioner med en aktiv distribution i primär region och en vänteläge distribution i säkerhetskopiering region. Dessutom nämnts toohello faktorer tidigare hello uppgraderingsprocessen måste garantera att:

* programmet hello förblir skyddat från allvarligt fel vid alla tidpunkter under uppgraderingsprocessen hello
* hello geo-redundant komponenter av programmet hello uppgraderas parallellt med hello active komponenter

tooachieve dessa mål som du använder Azure Traffic Manager (WATM) med hjälp av hello redundans profil med en aktiv och tre säkerhetskopierade slutpunkter.  hello följande diagram illustrerar hello driftsmiljön tidigare toohello uppgraderingsprocessen. Hej webbplatser <i>contoso 1.azurewebsites.net</i> och <i>contoso dr.azurewebsites.net</i> motsvarar en produktionsplatsen av programmet hello med fullständig geografisk redundans. tooenable hello möjlighet tooroll tillbaka Hej uppgraderingen, du måste skapa en steg-plats med en helt synkroniserad kopia av programmet hello. Eftersom du behöver tooensure hello program kan snabbt återställa om ett oåterkalleligt fel uppstår under uppgraderingsprocessen hello måste hello steget fack toobe geo-redundant samt. hello är följande steg nödvändiga tooprepare hello programmet hello uppgraderingen:

1. Skapa en plats för fas för hello uppgraderingen. toodo som skapar en sekundär databas (1) och distribuera en identisk kopia av hello-webbplats i hello samma Azure-region. Övervaka hello sekundära toosee om hello seeding processen har slutförts.
2. Skapa en geo-redundant sekundär databas i hello steget uttag av geo-replikering hello sekundära databasen toohello säkerhetskopiering region (Detta kallas ”härledda geo-replikering”). Övervaka hello säkerhetskopiering sekundära toosee om hello seeding processen är slutförd (3).
3. Skapa en vänteläge kopia av hello-webbplats i hello säkerhetskopiering region och länka det toohello geo-redundant sekundära (4).  
4. Lägga till ytterligare slutpunkter för hello <i>contoso 2.azurewebsites.net</i> och <i>contoso 3.azurewebsites.net</i> toohello redundans profil i WATM som offline slutpunkter (5). 

> [!NOTE]
> Obs hello förberedelsesteg påverkar inte programmet hello i hello produktionsplatsen och den kan fungera i full åtkomst-läge.
> 
> 

![Konfiguration av SQL Database geo-replikering. Katastrofåterställning i molnet.](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

När hello förberedelsesteg har slutförts, är hello steget klar för uppgradering av hello. hello följande diagram illustrerar hello uppgradering.

1. Ange hello primära databasen i hello fack endast tooread Produktionsläge (6). Detta garanterar att hello produktion instans av programmet hello (V1) förblir skrivskyddad uppgraderar hello vilket gör hello dataavvikelser mellan hello V1 och V2-databasinstanser.  
2. Koppla från hello sekundär databas i hello samma region med hello planerad avslutning läge (7). Den skapar en helt synkroniserad oberoende kopia av hello primära databasen, som blir automatiskt till en primär när hello avslutades. Den här databasen kommer att uppgraderas.
3. Aktivera hello primära databasen i hello steget fack tooread-och skrivbehörighet och kör hello uppgraderingsskriptet (8).    

![Konfiguration av SQL Database geo-replikering. Katastrofåterställning i molnet.](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Om hello uppgraderingen har slutförts men du är nu redo tooswitch hello slutanvändare toohello V2 version av programmet hello. hello följande diagram illustrerar hello stegen.

1. Växla hello aktiv slutpunkt i hello WATM profil för<i>contoso 2.azurewebsites.net</i>, som nu pekar toohello V2-versionen av hello-webbplats (9). Nu blir den en produktionsplatsen med hello V2 program och slutanvändarens trafiken dirigerad tooit. 
2. Om du inte längre behöver hello V1 program så att du kan på ett säkert sätt kan du ta bort den (10 och 11).  

![Konfiguration av SQL Database geo-replikering. Katastrofåterställning i molnet.](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Om hello uppgraderingsprocessen misslyckas, till exempel på grund av tooan fel i hello uppgraderingsskript, hello steget fack bör övervägas komprometteras. tooroll tillbaka hello toohello förberedande programtillstånd du bara återställa toousing hello programmet hello produktionsplatsen med fullständig åtkomst. hello stegen visas i nästa hello-diagrammet.    

1. Ange hello primära databaskopia hello produktion fack tooread skrivskyddad åtkomst (12). Detta återställer hello fullständig V1 funktionellt hello produktionsplatsen.
2. Utför hello Rotorsaksanalys och ta bort hello komprometteras komponenter i hello steget fack (13 och 14). 

Nu hello programmet fungerar fullt ut och hello Uppgraderingsstegen kan upprepas.

> [!NOTE]
> hello rollback inte kräver ändringar i WATM profilen eftersom den redan pekar för <i>contoso 1.azurewebsites.net</i> som hello aktiv slutpunkt.
> 
> 

![Konfiguration av SQL Database geo-replikering. Katastrofåterställning i molnet.](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

hello nyckeln **nytta** för det här alternativet är att du kan uppgradera både hello programmet och dess geo-redundant kopierar parallellt utan att kompromissa med din företagskontinuitet under hello uppgraderingen. hello huvudsakliga **förhållandet** är att den kräver dubbla redundans för varje PROGRAMKOMPONENTEN och därför högre dollar kostnad uppkommer för. Det innebär också en mer komplicerad arbetsflöde. 

## <a name="summary"></a>Sammanfattning
uppgradera hello två metoder som beskrivs i artikel hello skiljer sig åt i komplexitet och hello dollar kostnad men båda fokusera på Minimera hello tidpunkt då hello slutanvändaren är begränsad endast tooread-åtgärder. Då definieras direkt hello uppgraderingsskriptet hello varaktighet. Det är inte beroende hello databasstorleken, hello tjänstnivåmallen du valt, hello webbplatskonfiguration och andra faktorer som du kan enkelt styra. Detta beror på att alla hello förberedelsesteg frikopplat från hello uppgradering och kan göras utan att påverka hello produktionsprogram. hello effektiviteten för hello uppgraderingsskriptet är hello nyckelfaktor som bestämmer hello slutanvändarens upplevelse under uppgraderingar. Det är därför hello bästa sättet du kan förbättra den genom att fokusera arbetet på gör hello uppgraderingsskriptet så effektivt som möjligt.  

## <a name="next-steps"></a>Nästa steg
* En översikt över verksamhetskontinuitet och scenarier finns [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).
* toolearn om Azure SQL Database automatisk säkerhetskopiering finns [SQL-databas automatisk säkerhetskopiering](sql-database-automated-backups.md).
* toolearn om hur du använder automatisk säkerhetskopiering för återställning, se [återställa en databas från automatisk säkerhetskopiering](sql-database-recovery-using-backups.md).
* toolearn om snabbare återställningsalternativ finns [aktiv geo-replikering](sql-database-geo-replication-overview.md).


