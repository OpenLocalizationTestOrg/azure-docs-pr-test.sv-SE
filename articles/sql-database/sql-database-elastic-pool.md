---
title: "aaaWhat är elastiska pooler? Hantera flera SQL-databaser – Azure | Microsoft Docs"
description: "Hantera och skala flera databaser i SQL - hundratals och tusentalsavgränsare - med elastiska pooler. Ett pris för resurser som du kan distribuera om det behövs."
keywords: flera databaser, databasresurser, databasprestanda
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 07/31/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 2098d7817ebe1277b5c131421f23c00803ec78f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-sql-databases"></a>Hjälper dig att hantera och skala flera SQL-databaser för elastiska pooler

SQL-databas elastiska pooler är en enkel och kostnadseffektiv lösning för att hantera och skala flera databaser som har olika och oförutsägbart krav för användning. hello-databaserna i en elastisk pool finns på en enda Azure SQL Database-server och delar ett visst antal resurser ([elastiska Database Transaction Units](sql-database-what-is-a-dtu.md) (edtu: er)) till ett angivet pris. Elastiska pooler i Azure SQL Database aktivera SaaS utvecklare toooptimize hello pris prestanda för en grupp databaser i en föreskrivna budget leverera prestanda elasticitet för varje databas.   

> [!NOTE]
> Elastiska pooler är allmänt tillgängliga (GA) i alla Azure-regioner utom Västra Indien där de genomgår förhandsgranskning.  GA för elastiska pooler i den här regionen inträffar så snart som möjligt.
>

## <a name="what-are-sql-elastic-pools"></a>Vad är SQL elastiska pooler? 

SaaS-utvecklare utvecklar program på storskaliga datanivåer som består av flera databaser. En gemensam programmönster är tooprovision en enskild databas för varje kund. Men olika kunder har ofta olika och oförutsägbara användningsmönster, och det är svårt toopredict hello resurskraven för enskilda användare. Traditionellt har du två alternativ: 

- Etablera över resurser baserat på högsta användningsnivå och över lön, eller
- Under etablera toosave kostnaden vid hello kostnaden för prestanda och nöjda kunder under toppar. 

Elastiska pooler lösa detta problem genom att säkerställa som databaser får hello prestanda resurser som de behöver när de behöver. De tillhandahåller en enkel resursallokeringsmekanism med en förutsägbar budget. toolearn mer om designmönster för SaaS-program med elastiska pooler finns [designmönster för flera innehavare SaaS-program med Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Elastiska pooler aktivera hello developer toopurchase [elastiska Database Transaction Units](sql-database-what-is-a-dtu.md) (edtu: er) för en pool som delas av flera databaser tooaccommodate oförutsägbart perioder med användning av enskilda databaser. Hej eDTU krav för en pool bestäms av hello sammanställd användning av databaserna. hello antalet edtu: er tillgängliga toohello pool styrs av hello developer budget. hello utvecklare lägger bara till databaser toohello pool, anger hello lägsta och högsta edtu: er för hello databaser och anger hello eDTU för hello pool baserat på deras budget. En utvecklare kan använda pooler tooseamlessly växa tjänsten från ett lean Start tooa mogen på allt större skala.

Inom hello poolen ges enskilda databaser hello flexibilitet tooauto skala inom de angivna parametrarna. En databas kan använda flera edtu: er toomeet begäran vid hög belastning. Databaser med lätt arbetsbelastning förbrukar mindre, och databaser utan belastning förbrukar inga eDTU:er. Etablerar resurser för hello hela poolen i stället för enskilda databaser förenklar administrativa uppgifter. Plus du har en förutsägbar budget för hello poolen. Ytterligare edtu: er kan läggas till tooan befintlig pool utan avbrott i databasen, förutom att hello databaser behöva flytta toobe tooprovide hello ytterligare beräkningsresurser för hello nya eDTU reservation. På samma sätt kan eDTU:er som inte längre behövs tas bort från en befintlig pool när som helst. Och du kan lägga till eller ta bort databaser toohello pool. Om du vet att en databas underförbrukar resurser tar du bort den.

Du kan skapa och hantera en elastisk pool med hello [Azure-portalen](sql-database-elastic-pool-manage-portal.md), [PowerShell](sql-database-elastic-pool-manage-powershell.md), [Transact-SQL](sql-database-elastic-pool-manage-tsql.md), [C#](sql-database-elastic-pool-manage-csharp.md), och hello REST API. 

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>När bör du överväga en SQL Database-elastisk pool?

Pooler lämpar sig för ett stort antal databaser med specifika användningsmönster. För en viss databas kännetecknas det här mönstret av låg genomsnittlig användning med relativt ovanliga användningstoppar.

hello flera databaser du kan lägga till tooa pool hello större blir din besparingar. Beroende på din programmönster användning är det möjligt toosee besparingar med så lite som två S3 databaser.  

hello följande avsnitt hjälper dig att förstå hur tooassess om din specifika samling av databaser kan dra nytta av att den finns i en pool. hello exemplen använder Standard pooler men hello samma principer gäller även tooBasic och Premium-pooler.

### <a name="assessing-database-utilization-patterns"></a>Utvärdera databasanvändningsmönster

hello visar följande bild ett exempel på en databas som tillbringar mycket tid inaktiv, men också regelbundet ger spikar i diagrammet med aktivitet. Det här är ett användningsmönster som passar för en pool:

   ![en enkel databas som är lämplig för en pool](./media/sql-database-elastic-pool/one-database.png)

För hello fem minuter långa perioden visas, DB1 toppar in too90 dtu: er, men genomsnittliga användningen är mindre än 5 dtu: er. En S3 prestandanivå krävs toorun arbetsbelastningen i en enskild databas, men detta lämnar de flesta av hello resurser inte används under perioder med låg aktivitet.

En pool kan dessa oanvända dtu: er toobe som delas mellan flera databaser och minskar hello dtu: er behövs och den totala kostnaden.

Bygger på hello föregående exempel, anta att det finns ytterligare databaser med liknande användningsmönster som DB1. Hej användning av fyra databaserna i hello två figurerna nedan, och 20 databaser är lager till hello samma kurva tooillustrate hello icke-överlappande uppbyggnad deras belastning över tid:

   ![4 databaser med ett användningsmönster som passar för en pool](./media/sql-database-elastic-pool/four-databases.png)

  ![20 databaser med ett användningsmönster som passar för en pool](./media/sql-database-elastic-pool/twenty-databases.png)

hello sammanställd DTU-utnyttjande på alla 20 databaser illustreras av hello svart linje i hello föregående bild. Det visar att hello sammanställd DTU-utnyttjande aldrig överskrider 100 dtu: er och anger att hello 20 databaser kan dela 100 edtu: er för den här tidsperioden. Detta resulterar i 20 x minska dtu: er och en 13 x pris minskning jämfört med tooplacing varje hello databaser i S3 prestandanivåer för enskilda databaser.

Det här exemplet är idealisk för hello följande orsaker:

* Skillnaderna mellan användningen vid hög aktivitet och den genomsnittliga användningen per databas är stora.  
* hello belastning belastningen för varje databas inträffar vid olika tidpunkter i tid.
* eDTU:er som delas mellan flera databaser.

hello priset för poolen är en funktion av hello pool-edtu: er. Medan hello eDTU enhetspriset för en pool är 1,5 x större än hello DTU enhetspriset för en enskild databas **pool-edtu: er kan delas av många databaser och färre totala edtu: er som krävs för**. Dessa skillnader i priser och eDTU delning är hello grunden för hello pris besparingar potentiella pooler kan ge.  

hello relaterade följande tumregel toodatabase antal och databasen användning hjälp tooensure som levererar en pool minskar kostnaden jämfört med toousing prestandanivåer för enskilda databaser.

### <a name="minimum-number-of-databases"></a>Minsta antal databaser

Om hello summan av hello dtu: erna för prestandanivåer för enskilda databaser är mer än 1,5 x hello edtu: er som behövs för hello pool, är det mer kostnadseffektivt med en elastisk pool. Information om tillgängliga storlekar finns i avsnittet om [eDTU:er och lagringsgränser för elastiska pooler och elastiska databaser](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

***Exempel***<br>
Minst två S3 databaser eller minst 15 S0 databaser krävs för en 100 eDTU-pool toobe mer kostnadseffektivt än att använda prestandanivåer för enskilda databaser.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Högsta antal samtidigt databaser med aktivitetstoppar

Genom att dela edtu: er kan kan inte alla databaser i poolen samtidigt använda edtu: er in toohello begränsa tillgängliga när du använder prestandanivåer för enskilda databaser. Hej färre databaser som högsta samtidigt, hello lägre hello pool-eDTU kan ställas in och hello blir mer kostnadseffektiv hello-poolen. I allmänhet begränsa inte fler än 2/3 (eller 67%) av hello databaser i poolen hello bör samtidigt högsta tootheir eDTU.

***Exempel***<br>
tooreduce kostnader för tre S3 databaser i en 200 eDTU-pool, högst två av dessa databaser kan samtidigt högsta i deras användning. Om fler än två av dessa fyra S3 databaser högsta samtidigt, annars måste hello poolen toobe storlek toomore än 200 edtu: er. Om hello poolen är storleksändrade toomore än 200 edtu: er, behöver mer S3 databaser toobe som lagts till toohello pool tookeep kostnader lägre än prestandanivåer för enskilda databaser.

Observera användning av andra databaser i poolen hello inte beaktas i det här exemplet. Om alla databaser har vissa användning vid en viss tidpunkt, sedan mindre än 2/3 (eller 67%) av hello databaser kan högsta samtidigt.

### <a name="dtu-utilization-per-database"></a>DTU-användning per databas
En stor skillnad mellan hello belastning och genomsnittlig användning av en databas anger långvarig perioder med låg belastning och korta perioder med hög belastning. Det här användningsmönstret är idealisk för delning av resurser mellan databaser. Du bör överväga att lägga till en databas i en pool om dess högsta användning är runt 1,5 gånger större än dess genomsnittliga användning.

***Exempel***<br>
S3-databas som toppar som pekar åt too100 dtu: er och använder i genomsnitt 67 dtu: er eller mindre är en bra kandidat för delning av edtu: er i poolen. Du kan också en S1-databas som toppar som pekar åt too20 dtu: er och använder i genomsnitt 13 dtu: er eller mindre är en bra kandidat för en pool.

## <a name="how-do-i-choose-hello-correct-pool-size"></a>Hur väljer hello rätt poolstorlek?

hello bästa storleken för en pool beror på hello sammanställd edtu: er och storage-resurser som krävs för alla databaser i hello pool. Detta omfattar att fastställa hello större hello följande:

* Maximal dtu: er används av alla databaser i hello pool.
* Maximalt lagringsutrymme byte som används av alla databaser i hello pool.

Information om tillgängliga storlekar finns i avsnittet om [eDTU:er och lagringsgränser för elastiska pooler och elastiska databaser](#what-are-the-resource-limits-for-elastic-pools).

SQL-databasen automatiskt utvärderar hello historiska resursanvändningen för databaser i en befintlig SQL-databasserver och rekommenderar hello poolen konfigurationen i hello Azure-portalen. I tillägg toohello rekommendationer beräknar en inbyggd upplevelse hello eDTU-användning för en anpassad grupp av databaser på hello-server. Detta gör toodo en ”” konsekvensanalys av interaktivt lägga till databaser toohello poolen och ta bort dem tooget resurs användningsanalys och storleksanpassa råd innan du genomför ändringarna. Mer information finns i [Monitor, manage, and size an elastic pool](sql-database-elastic-pool-manage-portal.md) (Övervaka, hantera och ändra storlek på en elastisk pool).

I fall där du inte kan använda verktygsuppsättning hjälper hello följande steg för steg dig att beräkna om poolen är mer kostnadseffektivt än enskilda databaser:

1. Beräkna hello edtu: er som behövs för hello poolen på följande sätt:

   MAX(<*totalt antal databaser* × *genomsnittlig DTU-användning per databas*>,<br>
   <*Antal databaser som har aktivitetstoppar samtidigt* × *DTU-toppbelastning per databas*)
2. Beräkna hello lagringsutrymme som krävs för hello poolen genom att lägga till hello antalet byte som krävs för alla hello databaser i hello pool. Sedan fastställa hello eDTU poolstorlek som innehåller den här mängden lagringsutrymme. Information om gränser för poollagring baserat på eDTU-poolstorlek finns i [eDTU and storage limits for elastic database pools and elastic databases](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) (eDTU:er och lagringsgränser för elastiska pooler och elastiska databaser).
3. Ta hello större hello eDTU uppskattningar från steg 1 och 2.
4. Se hello [SQL-databas sida med priser](https://azure.microsoft.com/pricing/details/sql-database/) och hitta eDTU-pool med hello minsta storlek som är större än hello uppskattning från steg3.
5. Jämför hello poolen price från steg 5 toohello priset för att använda hello lämpliga prestandanivåer för enskilda databaser.

### <a name="changing-elastic-pool-resources"></a>Ändra elastisk pool resurser

Du kan öka eller minska hello resurser tillgängliga tooan elastisk pool baserat på resursbehov.

* Ändra hello min edtu: er per databas eller max edtu: er per databas vanligtvis har slutförts i 5 minuter eller mindre.
* Ändra hello edtu: er per pool beror på hello totala mängden diskutrymme som används av alla databaser i hello pool. Ändringarna tar i genomsnitt 90 minuter eller mindre per 100 GB. Om hello totalt utrymme som används av alla databaser i poolen hello är exempelvis 200 GB, och sedan hello förväntade svarstid för att ändra hello pool-eDTU per pool är tre timmar eller mindre.

## <a name="what-are-hello-resource-limits-for-elastic-pools"></a>Vad är hello gränserna för elastiska pooler?

hello följande tabeller beskrivs hello gränserna för elastiska pooler.  Observera att hello gränserna för enskilda databaser i elastiska pooler är vanligtvis hello desamma som för enskilda databaser utanför pooler baserat på dtu: er och hello tjänstnivån.  Hej max samtidiga arbetare för en S2-databas är till exempel 120 arbetare.  Hej max samtidiga arbetare för en databas i en Standardpool är därför också 120 arbetare om Hej max DTU per databas i hello poolen är 50 dtu: er (som är likvärdiga tooS2).

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Om alla dtu: er för en elastisk pool, får varje databas i hello pool lika mycket resurser tooprocess frågor.  hello SQL Database-tjänsten tillhandahåller resursdelning skälighet mellan databaser genom att säkerställa lika kortare perioder för beräkning. Elastisk pool resursdelning skälighet är dessutom tooany resurs annars garanteras tooeach databasen när minsta för hello DTU per databas är värdet tooa inte är noll.

### <a name="database-properties-for-pooled-databases"></a>Databasegenskaper för grupperade databaser

hello i den följande tabellen beskrivs hello egenskaper för grupperade databaser.

| Egenskap | Beskrivning |
|:--- |:--- |
| Maximalt antal eDTU:er per databas |hello maximala antalet edtu: er som alla databaser i poolen hello kan använda om tillgängliga baserat på användning av andra databaser i hello pool.  Det högsta antalet eDTU:er per databas utgör ingen resursgaranti för en databas.  Den här inställningen är en global inställning som gäller tooall databaser i hello pool. Ange max edtu: er per databas tillräckligt högt toohandle toppar i databasen användning. Vissa graden av överanstränger förväntas eftersom hello poolen vanligtvis förutsätter varm eller kall användningsmönster för databaser där alla databaser inte samtidigt peaking. Anta exempelvis att hello belastning användning per databas är 20 edtu: er och bara 20% av hello 100 databaser i poolen hello belastning på hello samtidigt.  Om hello eDTU max per databas är too20 edtu: er, är rimliga tooovercommit hello pool av 5 gånger och ange hello edtu: er per pool too400. |
| Minimalt antal eDTU:er per databas |hello minsta antal edtu: er som alla databaser i poolen hello garanterat.  Den här inställningen är en global inställning som gäller tooall databaser i hello pool. hello min eDTU per databas får anges too0 och är också hello standardvärdet. Den här egenskapen anges tooanywhere mellan 0 och hello genomsnittlig eDTU-användning per databas. hello produkten av hello antal databaser i poolen hello och hello min edtu: er per databas får inte överskrida hello edtu: er per pool.  Till exempel om poolen har 20 databaser och hello eDTU min per databas too10 edtu: er kan måste sedan hello edtu: er per pool vara minst lika stor som 200 edtu: er. |
| Maximalt datalagringsutrymme per databas |hello maximalt lagringsutrymme för en databas i en pool. Grupperade databaser dela lagringsutrymme för pool så databaslagring är begränsad toohello mindre lagringsutrymme för återstående pool och maximal lagringskapacitet per databas. Maximalt lagringsutrymme per databas refererar toohello maxstorleken för hello datafiler och innehåller inte hello utrymme som används av loggfiler. |
|||

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Använda andra funktioner i SQL-databas med elastiska pooler

### <a name="elastic-jobs-and-elastic-pools"></a>Elastiska jobb och elastiska pooler

Med en pool förenklas hanteringsuppgifterna eftersom du kan köra skript i **[elastiska jobb](sql-database-elastic-jobs-overview.md)**. Ett elastiskt jobb underlättar hanteringen av stora antal databaser. toobegin, se [komma igång med elastiska jobb](sql-database-elastic-jobs-getting-started.md).

Mer information om andra databasverktyg för att jobba med flera databaser, finns i [Skala ut med Azure SQL Database](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Alternativ för verksamhetskontinuitet för databaser i en elastisk pool
Pooler databaser vanligtvis stöd hello samma [funktionerna för verksamhetskontinuitet](sql-database-business-continuity.md) som är tillgängliga toosingle databaser.

- **Återställning vid tidpunkt**: punkt i tiden återställning använder automatisk databasen säkerhetskopieringar toorecover en databas i en pool tooa punkt i tiden. Mer information finns i avsnittet om [återställning till tidpunkt](sql-database-recovery-using-backups.md#point-in-time-restore)

- **GEO-återställning**: Geo-återställning ger hello standardalternativet när en databas är inte tillgänglig på grund av en incident i hello region där hello-databasen finns. Se [återställa en Azure SQL Database eller failover tooa sekundär](sql-database-disaster-recovery.md)

- **Aktiv geo-replikering**: konfigurera för program som har mer aggressivt recovery krav än geo-återställning kan erbjuda [aktiv geo-replikering](sql-database-geo-replication-overview.md).

## <a name="manage-sql-database-elastic-pools-using-hello-azure-portal"></a>Hantera SQL Database: elastiska pooler med hello Azure-portalen

### <a name="creating-a-new-sql-database-elastic-pool-using-hello-azure-portal"></a>Skapa en ny SQL Database-elastisk pool med hello Azure-portalen

Det finns två sätt som du kan skapa en elastisk pool i hello Azure-portalen. Du kan göra det från grunden om du vet hello pool-installationen du vill eller börja med en rekommendation från hello-tjänsten. SQL Database har inbyggd intelligens som rekommenderar en elastisk pool-installationen om det är mer kostnadseffektivt baserat på hello senaste användningstelemetrin för dina databaser. 

Skapa en elastisk pool från en befintlig **server** bladet i hello portal är hello enklaste sättet toomove befintliga databaser till en elastisk pool. Du kan också skapa en elastisk pool genom att söka **elastiska SQL-poolen** i hello **Marketplace** eller klicka på **+ Lägg till** i hello **SQL elastiska pooler**Bläddra bladet. Du kan kan toospecify en ny eller befintlig server via den här poolen etablering av arbetsflöde.

> [!NOTE]
> Du kan skapa flera pooler på en server, men du kan inte lägga till databaser från olika servrar i hello samma pool.
>  

hello avgör prisnivån för poolen hello funktioner tillgängliga toohello elastics i hello poolen och hello maximala antalet edtu: er (eDTU MAX) och lagringsutrymme (GB) tillgängliga tooeach databas. Mer information finns i [tjänstnivåer](#edtu-and-storage-limits-for-elastic-pools).

toochange hello prisnivå för poolen hello, klickar du på **prisnivå**, klicka på hello prisnivån och klickar sedan på **Välj**.

> [!IMPORTANT]
> När du väljer hello prisnivån och genomför ändringarna genom att klicka på **OK** hello sista steget, inte kan toochange hello prisnivån för hello pool. toochange hello prisnivå för en befintlig elastisk pool, skapa en elastisk pool i hello önskade prisnivån och migrera hello databaser toothis ny pool.
>

Om hello databaser du arbetar med har tillräckligt med historisk användningstelemetri, hello **beräknad eDTU och GB-användning** diagram och hello **faktisk eDTU-användning** stapeldiagram uppdatering toohelp du göra konfiguration beslut. Dessutom du hello-tjänsten kan ge en rekommendation meddelandet toohelp du rätt storlek hello poolen.

hello SQL Database-tjänsten utvärderar Användningshistorik och rekommenderar en eller flera pooler när det är mer kostnadseffektivt än att använda enskilda databaser. Varje rekommendation är konfigurerad med en unik delmängd av hello server-databaser som bäst passar hello poolen.

![rekommenderad pool](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

hello pool-rekommendationerna omfattar:

- En prisnivå för hello poolen (Basic, Standard, Premium eller Premium RS)
- Lämpliga **POOL-eDTU:er** (kallas även Max eDTU:er per pool)
- Hej **eDTU MAX** och **eDTU Min** per databas
- hello listan över rekommenderade databaser för hello-pool

> [!IMPORTANT]
> hello service beaktas hello senaste 30 dagarnas telemetri vid rekommendation av pooler. För en databas toobe betraktas som en kandidat för en elastisk pool, måste det finnas minst 7 dagar. Databaser som redan finns i en elastisk pool är inte rekommenderade kandidater för elastiska pooler.
>

hello tjänsten utvärderar resursbehov och kostnadseffektivitet av glidande hello enda databaser i varje tjänstnivå till pooler inom hello samma nivå. Alla standard-databaser på servern utvärderas exempelvis för hur de skulle passa in i en standard elastisk pool. Det innebär att hello-tjänsten inte göra rekommendationer mellan olika nivåer, till exempel flytta en Standard-databas till en Premium-pool.

När du lägger till databaser toohello pool genereras dynamiskt rekommendationer baserat på hello historisk användning av hello-databaser som du har valt. De här rekommendationerna som visas i hello eDTU och GB och i en rekommendations-banderoll överst hello i hello **konfigurera pool** bladet. De här rekommendationerna är avsedda tooassist som du skapar en elastisk pool optimerad för dina specifika databaser.

![dynamiska rekommendationer](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

### <a name="manage-and-monitor-an-elastic-pool"></a>Hantera och övervaka en elastisk pool

Du kan övervaka hello-belastningen för en elastisk pool och hello databaser i poolen i hello Azure-portalen. Du kan också göra en uppsättning ändringar tooyour elastisk pool och skicka alla ändringar på hello samma tid. De här förändringarna innefattar att lägga till eller ta bort databaser, ändra inställningarna elastisk pool eller ändra databasinställningarna.

hello följande bild visar ett exempel elastisk pool. hello-vyn innehåller:

*  Diagram för övervakning av resursanvändningen för både hello elastisk pool och hello databaser som ingår i hello pool.
*  Hej **konfigurera** pool knappen toomake ändras toohello elastisk pool.
*  Hej **Skapa databas** skapar du en databas och lägger till den toohello aktuella elastisk pool.
*  Elastiska jobb som hjälper dig hantera stort antal databaser genom att köra Transact-SQL-skript mot alla databaser i en lista.

![Pool-vy](./media/sql-database-elastic-pool-manage-portal/basic.png)

Du kan gå tooa viss poolen toosee dess resursutnyttjande. Hello poolen är som standard konfigurerade tooshow lagring och eDTU-användning för hello senaste timmen. hello diagrammet kan vara konfigurerade tooshow olika mätvärden via olika tidsfönster. Klicka på hello **resursutnyttjande** diagram **elastisk pool övervakning** tooshow en detaljerad vy av hello angetts mätvärden via hello angivna tidsfönstret.

![Övervakning av elastisk pool](./media/sql-database-elastic-pool-manage-portal/basic-2.png)

![Bladet Mått](./media/sql-database-elastic-pool-manage-portal/metric.png)

### <a name="toocustomize-hello-chart-display"></a>toocustomize hello diagramvyn

Du kan redigera hello diagram och hello mått bladet toodisplay andra mått som CPU-procent, data IO-procent och loggen IO-procent som används.

![Klicka på Redigera](./media/sql-database-elastic-pool-manage-portal/edit-metric.png)

På hello **redigera diagram** formuläret, du kan välja ett tidsintervall (efter timme, dag, eller föregående vecka), eller klicka på **anpassade** tooselect i hello vara ett datum senaste två veckorna. Du kan välja mellan ett fält eller ett linjediagram och välj sedan hello resurser toomonitor.

> [!Note]
> Endast mått med samma enhet som kan visas i hello hello diagram på hello samma tid. Till exempel om du väljer ”eDTU i procent” kan du bara välja andra mått med procentandel som hello enhet.
>

[Klicka på Redigera](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

### <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Hantera och övervaka databaser i en elastisk pool

Enskilda databaser kan också övervakas för potentiella problem. Under **elastisk databas övervakning**, det finns ett diagram som visar mått för fem databaser. Som standard hello diagrammet visar hello övre 5 databaser i poolen hello av genomsnittlig eDTU-användning i hello senaste timmen. 

![Övervakning av elastisk pool](./media/sql-database-elastic-pool-manage-portal/basic-3.png)

Klicka på hello **eDTU-användning för databaser för hello senaste timmen** under **elastisk databas övervakning**. Då öppnas **databasen resursutnyttjande** och ger en detaljerad vy av hello databasanvändningen i hello pool. Använder hello rutnät i hello nedre delen av hello-bladet, kan du välja alla databaser i hello poolen toodisplay användningen i hello diagram (upp too5 databaser). Du kan också anpassa hello mått och tid fönster som visas i diagrammet hello genom att klicka på **redigera diagram**.

![-Databasresursbladet användning](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="toocustomize-hello-view"></a>toocustomize hello vy

Du kan redigera hello diagram tooselect tidsintervall (efter timme eller senaste 24 timmarna) eller klicka på **anpassade** tooselect en annan dag i hello efter två veckor toodisplay.

![Klicka på Redigera diagram](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

![Klicka på anpassad](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

Du kan också klicka på hello **databaser genom att jämföra** listrutan tooselect olika mått toouse när databaser.

![Redigera hello diagram](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="tooselect-databases-toomonitor"></a>tooselect databaser toomonitor

I listan för hello databasen hello **databasen resursutnyttjande** bladet hittar du viss databaser genom att titta igenom hello sidorna i hello listan eller genom att skriva hello namnet på en databas. Använd hello kryssrutan tooselect hello-databasen.

![Sök efter databaser toomonitor](./media/sql-database-elastic-pool-manage-portal/select-dbs.png)


### <a name="add-an-alert-tooan-elastic-pool-resource"></a>Lägg till en resurs för avisering tooan elastisk pool

Du kan lägga till regler tooan elastisk pool som skickar e-post toopeople eller varning strängar tooURL slutpunkter när hello elastisk pool träffar ett tröskelvärde för användning som du skapat.

**tooadd en avisering tooany resurs:**

1. Klicka på hello **resursutnyttjande** diagram tooopen hello **mått** bladet, klickar du på **Lägg till avisering**, och fyller sedan hello informationen i hello **lägga till en avisering regeln** bladet (**resurs** konfigureras automatiskt toobe hello pool du arbetar med).
2. Ange en **namn** och **beskrivning** som identifierar hello avisering tooyou och hello mottagare.
3. Välj en **mått** som du vill tooalert hello-listan.

    hello diagrammet visar dynamiskt resursanvändningen för den mått toohelp du väljer ett tröskelvärde.

4. Välj en **villkoret** (större än mindre än osv) och en **tröskelvärdet**.
5. Välj en **Period** som hello mått regeln måste uppfyllas innan hello avisering utlösare.
6. Klicka på **OK**.

Mer information finns i [skapa SQL-databas aviseringar i Azure-portalen](sql-database-insights-alerts-portal.md).

### <a name="move-a-database-into-an-elastic-pool"></a>Flytta en databas till en elastisk pool

Du kan lägga till eller ta bort databaser från en befintlig adresspool. hello databaser kan vara i andra pooler. Men du kan bara lägga till databaser som finns på hello samma logiska server.

 ![Klicka på Konfigurera pool](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

![Klicka på Lägg till toopool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)

![Välj databaser tooadd](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

![Väntande pool-tillägg](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="move-a-database-out-of-an-elastic-pool"></a>Flytta en databas från en elastisk pool

![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

![Visar en lista över databaser](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

![Förhandsgranska databasen tillägg och borttagning](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

![Klicka på Spara](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="change-performance-settings-of-an-elastic-pool"></a>Ändra inställningar för prestanda för en elastisk pool

När du övervakar hello resursanvändningen för en elastisk pool kan du märka att några justeringar behövs. Hello poolen behöver kanske en ändring i hello prestanda eller lagring gränser. Du vill eventuellt toochange hello databasinställningarna i hello pool. Du kan ändra hello installationen av hello poolen på varje gång tooget hello bästa balansen mellan prestanda och kostnad. Se [när en elastisk pool kan användas?](sql-database-elastic-pool.md) för mer information.

toochange hello edtu: er eller lagring gränser per pool och edtu: er per databas:

![Resursutnyttjande elastisk pool](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

![Uppdatera en elastisk pool och nya månadskostnaden](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="manage-sql-database-elastic-pools-using-powershell"></a>Hantera SQL Database: elastiska pooler med hjälp av PowerShell

toocreate och hantera SQL-databas elastiska pooler med Azure PowerShell, Använd hello följande PowerShell-cmdlets. Om du behöver tooinstall eller uppgradera PowerShell Se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps). toocreate och hantera databaser, servrar och brandväggsregler, se [skapa och hantera Azure SQL Database-servrar och databaser med hjälp av PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell). 

> [!TIP]
> PowerShell-exempelskript, se [skapa elastiska pooler och flytta databaser mellan pooler och från en pool med PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) och [Använd PowerShell toomonitor och skala en SQL-elastisk pool i Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Beskrivning |
| --- | --- |
|[Ny AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Skapar en elastisk databaspool på en logisk SQLServer.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Hämtar elastiska pooler och deras värden på en logisk SQLServer.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Ändrar egenskaper för en elastisk databaspool på en logisk SQLServer.|
|[Ta bort AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Tar bort en elastisk databaspool på en logisk SQLServer.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Hämtar hello status för åtgärder på en elastisk pool på en logisk SQLServer.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Skapar en ny databas i en befintlig adresspool eller som en enskild databas. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Hämtar en eller flera databaser.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Anger egenskaperna för en databas eller flyttar en befintlig databas i, slut på eller mellan elastiska pooler.|
|[Ta bort-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Tar bort en databas.|

> [!TIP]
> Skapandet av många databaser i en elastisk pool kan ta tid när det görs med hello portal eller PowerShell-cmdletar som skapar bara en enskild databas åt gången. tooautomate skapande i en elastisk pool, se [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-sql-database-elastic-pools-using-hello-azure-cli"></a>Hantera SQL Database: elastiska pooler med hello Azure CLI

toocreate och hantera SQL-databas elastiska pooler med hello [Azure CLI](/cli/azure/overview), använder hello följande [Azure CLI SQL Database](/cli/azure/sql/db) kommandon. Använd hello [moln Shell](/azure/cloud-shell/overview) toorun hello CLI i webbläsaren eller [installera](/cli/azure/install-azure-cli) på macOS, Linux och Windows. 

> [!TIP]
> Azure CLI exempelskript finns [Använd CLI toomove en Azure SQL database i en elastisk pool SQL](scripts/sql-database-move-database-between-pools-cli.md) och [Använd Azure CLI tooscale en elastisk SQL-pool i Azure SQL Database](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Beskrivning |
| --- | --- |
|[Skapa AZ sql elastisk pool](/cli/azure/sql/elastic-pool#create)|Skapar en elastisk pool.|
|[AZ sql elastisk pool lista](/cli/azure/sql/elastic-pool#list)|Returnerar en lista över elastiska pooler i en server.|
|[AZ sql elastisk pool lista-databaser](/cli/azure/sql/elastic-pool#list-dbs)|Returnerar en lista över databaser i en elastisk pool.|
|[AZ sql elastisk pool lista-versioner](/cli/azure/sql/elastic-pool#list-editions)|Även tillgängliga poolen DTU inställningar, Lagringsgränser och per databasinställningarna. Inställningar för döljs som standard i ordning tooreduce detaljnivå ytterligare Lagringsgränser och per databas.|
|[AZ sql elastisk pool uppdateringen](/cli/azure/sql/elastic-pool#update)|Uppdaterar en elastisk pool.|
|[ta bort AZ sql-elastisk pool](/cli/azure/sql/elastic-pool#delete)|Tar bort hello elastisk pool.|

## <a name="manage-sql-database-elastic-pools-using-transact-sql"></a>Hantera SQL Database: elastiska pooler med Transact-SQL

toocreate och flytta databaser i befintliga elastiska pooler eller tooreturn information om en SQL Database-elastisk pool med Transact-SQL, Använd hello följande T-SQL-kommandon. Du kan skicka dessa kommandon använder hello Azure-portalen [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), eller andra program som kan ansluta tooan Azure SQL Database-server och skicka Transact-SQL kommandon. toocreate och hantera databaser, servrar och brandväggsregler, se [skapa och hantera Azure SQL Database-servrar och databaser med hjälp av Transact-SQL](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Du kan inte skapa, uppdatera eller ta bort en Azure SQL Database-elastisk pool med Transact-SQL. Du kan lägga till eller ta bort databaser från en elastisk pool och du kan använda av DMV: er tooreturn information om befintliga elastiska pooler.
>

| Kommando | Beskrivning |
| --- | --- |
|[Skapa databas (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Skapar en ny databas i en befintlig adresspool eller som en enskild databas. Du måste vara anslutna toohello huvuddatabasen toocreate en ny databas.|
| [ALTER DATABASE (Azure SQL-databas)](/sql/t-sql/statements/alter-database-azure-sql-database) |Flytta en databas i, slut på eller mellan elastiska pooler.|
|[Ta bort databasen (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Tar bort en databas.|
|[sys.elastic_pool_resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Returnerar resurs användningsstatistik för alla hello elastiska databaspooler i en logisk server. För varje elastisk databaspool att det finns en rad för varje 15 sekunder reporting fönstret (fyra rader per minut). Detta inkluderar CPU, IO, Log, användningen av lagringsutrymme och samtidiga begäran-session användning alla databaser i hello pool.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Returnerar hello edition (tjänstnivån), tjänstmålet (prisnivån) och namn på elastisk pool, för en Azure SQL-databas eller ett Azure SQL Data Warehouse. Returnerar information om alla databaser om inloggad toohello master-databasen i en Azure SQL Database-server. Du måste vara anslutna toohello huvuddatabasen för Azure SQL Data Warehouse.|

## <a name="manage-sql-database-elastic-pools-using-hello-rest-api"></a>Hantera SQL Database: elastiska pooler med hello REST API

toocreate och hantera SQL Database: elastiska pooler med hello REST-API, se [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Nästa steg

* Se en video [Microsoft Virtual Academy video kursen på Azure SQL Database-elastisk funktioner](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* toolearn mer om designmönster för SaaS-program med elastiska pooler finns [designmönster för flera innehavare SaaS-program med Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* Om du har en SaaS-självstudiekurs med elastiska pooler finns [introduktion toohello Wingtip SaaS-program](sql-database-wtp-overview.md).
