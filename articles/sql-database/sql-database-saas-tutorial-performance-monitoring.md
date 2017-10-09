---
title: "aaaMonitor prestanda i många Azure SQL-databaser i en app för flera innehavare SaaS | Microsoft Docs"
description: "Övervaka och hantera prestanda för databaser och pooler i hello Azure SQL Database Wingtip SaaS-app"
keywords: sql database tutorial
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: sstein
ms.openlocfilehash: f0d7ba456c485b7de249a56abac3cf4be3857285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-performance-of-hello-wingtip-saas-application"></a>Övervaka prestanda för hello Wingtip SaaS-program

I den här självstudiekursen beskrivs flera KPI hanteringsscenarier som används i SaaS-program. Med hjälp av en load generator toosimulate aktivitet mellan alla klient-databaser är hello inbyggd övervakning och aviseringar funktioner i SQL-databasen och elastiska pooler visas.

hello Wingtip SaaS appen använder en enskild klient datamodellen, där varje plats (klient) har sina egna databasen. Liksom många SaaS-program förväntad hello klient arbetsbelastning mönstret är oförutsägbart och sporadiska. Biljettförsäljningar kan med andra ord ske när som helst. tootake nytta av den här databasen användningsmönstret klient databaser har distribuerats till elastiska databaspooler. Elastiska pooler optimera hello kostnaden för en lösning genom att dela resurser över flera databaser. Med den här typen av mönstret är det viktigt toomonitor databasen och pool resurs användning tooensure som läser in balanseras rimligen över pooler. Du måste också tooensure att enskilda databaser har tillräckliga resurser och att pooler inte träffa sina [eDTU](sql-database-what-is-a-dtu.md) gränser. Den här självstudiekursen utforskar sätt toomonitor och hantera databaser och pooler, och hur tootake korrigerande åtgärder i svaret toovariations i arbetsbelastning.

I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]

> * Simulera användning på hello klient databaser genom att köra ett angivet belastningen generator
> * Övervakaren hello klient databaser som de svara toohello ökad belastning
> * Skala upp hello elastiska poolen i svaret toohello ökad databasen belastning
> * Etablera en andra Databasaktivitet elastisk pool tooload saldo


den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:

* hello Wingtip SaaS appen har distribuerats. toodeploy på mindre än fem minuter finns [distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)
* Azure PowerShell ska ha installerats. Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-toosaas-performance-management-patterns"></a>Introduktion tooSaaS prestanda management mönster

Hantera databasprestanda består av sammanställa och analysera prestandadata och sedan reagerar toothis data genom att justera parametrarna toomaintain acceptabel svarstid för ditt program. När värd för flera innehavare, elastiska databaspooler är ett kostnadseffektivt sätt tooprovide och hantera resurser för en grupp databaser med oförutsägbart arbetsbelastningar. Med vissa arbetsbelastningmönster, kan så få som två S3-databaser dra nytta av att hanteras i en pool.

![media](./media/sql-database-saas-tutorial-performance-monitoring/app-diagram.png)

Pooler och hello databaser i poolen, bör vara övervakade tooensure de förblir inom acceptabel prestanda. Finjustera hello poolen configuration toomeet hello behov hello sammanställd arbetsbelastningen för alla databaser att säkerställa att hello pool-edtu: er är lämplig för hello övergripande arbetsbelastning. Justera hello per databas min och per databas max eDTU värden tooappropriate värden för din specifika programkrav.

### <a name="performance-management-strategies"></a>Strategier för prestandahantering

* tooavoid med toomanually övervaka prestanda, det är bäst för**ställa in aviseringar som utlöses när databaser eller pooler avvika utanför normal adressintervall**.
* toorespond tooshort termen variationer i hello sammanställd prestandanivå en adresspool hello **pool-eDTU nivå kan skalas upp eller ned**. Om den här variationerna inträffar på grundval regelbundna eller förutsägbar **skalning hello pool kan vara schemalagda toooccur automatiskt**. Du kan exempelvis skala ned när du vet att din arbetsbelastning är lätt, på nätter eller helger till exempel.
* toorespond toolonger termen variationer eller ändringar i hello antalet databaser, **enskilda databaser kan flyttas till andra pooler**.
* toorespond tooshort termen ökar i *enskilda* databasbelastningen **enskilda databaser kan tas bort från en pool och tilldelats en enskilda prestandanivå**. När hello belastningen minskar, kan hello databasen sedan returneras toohello pool. När det är känt i förväg, kan databaser flyttas pre-emptively tooensure hello databasen har alltid hello resurser som behövs och tooavoid inverkan på andra databaser i poolen hello. Om det här kravet är förutsägbar, till exempel en plats som har en går lite för snabbt för Biljettförsäljning för en populär händelse, kan problemet management integreras i hello program.

Hej [Azure-portalen](https://portal.azure.com) innehåller inbyggd övervakning och avisering för de flesta resurser. För SQL Database, finns övervakning och avisering tillgängligt för databaser och pooler. Den här inbyggda övervakning och avisering är resursspecifika, så det är praktiskt toouse för litet antal resurser, men är inte är användbart när du arbetar med många resurser.

För omfattande scenarier där du arbetar med många reources [logganalys (OMS)](sql-database-saas-tutorial-log-analytics.md) kan användas. Detta är en separat Azure-tjänst som ger analytics över skickade diagnostikloggar och telemetri som samlats in i en log analytics-arbetsyta. Logganalys kan samla in telemetri från många tjänster och använda tooquery och Ställ in aviseringar.

## <a name="get-hello-wingtip-application-source-code-and-scripts"></a>Hämta hello Wingtip programmets källkod och skript

hello Wingtip SaaS-skript och programmets källkod är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen. [Steg toodownload hello Wingtip SaaS skript](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="provision-additional-tenants"></a>Etablera ytterligare klienter

Pooler kan vara kostnadseffektiva med två S3 databaser, hello flera databaser som är i hello poolen hello mer kostnadseffektivt hello medelvärdet effekten blir. För att få en bra förståelse för hur prestandaövervakning och hantering fungerar i stor skala, kräver den här kursen du har distribuerat minst 20 databaser.

Om du redan har etablerats en grupp med klienter i en tidigare självstudierna kan du hoppa över toohello [simulera användning för alla databaser som klient](#simulate-usage-on-all-tenant-databases) avsnitt.

1. Öppna... \\Learning moduler\\prestandaövervakning och hantering av\\*Demo-PerformanceMonitoringAndManagement.ps1* i hello *PowerShell ISE*. Ha det här skriptet öppet medan du kör scenarierna i den här guiden.
1. Ställ in **$DemoScenario** = **1**, **etablera en batch med klienter**
1. Tryck på **F5** toorun hello skript.

hello skript distribuerar 17 klienter på mindre än fem minuter.

Hej *ny TenantBatch* skript som använder en kapslad eller länkade uppsättning [Resource Manager](../azure-resource-manager/index.md) mallar som skapar en grupp med klienter, vilket som standard kopierar hello databasen **basetenantdb** på hello katalog server toocreate hello ny klient databaser, registrerar sedan dessa i hello katalog och slutligen initierar dem med hello klient namn och plats. Detta är förenligt med hello sätt hello app etablerar en ny klient. Ändringar som görs för*basetenantdb* är tillämpade tooany nya klienter som etablerats därefter. Se hello [schemat Management kursen](sql-database-saas-tutorial-schema-management.md) toosee hur toomake schemat ändras för*befintliga* klient databaser (inklusive hello *basetenantdb* databas).

## <a name="simulate-usage-on-all-tenant-databases"></a>Simulera användning på alla klientdatabaser

Hej *Demo-PerformanceMonitoringAndManagement.ps1* skript har angetts som simulerar en arbetsbelastning som körs mot alla klient-databaser. hello belastningen skapas med någon av hello tillgängliga belastningen scenarier:

| Demo | Scenario |
|:--|:--|
| 2 | Generera belastning av normal intensitet (ca 40 DTU) |
| 3 | Generera belastning med längre och mer frekventa toppar per databas|
| 4 | Generera belastningar med högre DTU-toppar per databas (cirka 80 DTU)|
| 5 | Generera en normal belastning plus en hög belastning på en enskild klient (cirka 95 DTU)|
| 6 | Generera obalanserad belastning över flera pooler|

hello belastningen generator gäller en *syntetiska* endast CPU-belastningen tooevery klient-databasen. hello generator startar ett jobb för varje klient-databasen, som anropar en lagrad procedur med jämna mellanrum som genererar hello belastningen. hello belastningsnivåer (i edtu: er), varaktighet och intervall varieras över alla databaser som simulerar oförutsägbart klient aktivitet.

1. Öppna... \\Learning moduler\\prestandaövervakning och hantering av\\*Demo-PerformanceMonitoringAndManagement.ps1* i hello *PowerShell ISE*. Ha det här skriptet öppet medan du kör scenarierna i den här guiden.
1. Ange **$DemoScenario** = **2**, *generera normal styrka belastningen*.
1. Tryck på **F5** tooapply en belastningen tooall klient-databaser.

Wingtip är en SaaS-app och hello verkliga belastningen på en SaaS-app är vanligtvis sporadiska och oförutsägbart. toosimulate detta hello belastningen generator ger en slumpmässig belastning fördelad över alla klienter. Flera minuter krävs för hello belastningen mönster tooemerge, så kör hello belastningen generatorn för 3-5 minuter innan du försöker toomonitor hello belastningen i hello följande avsnitt.

> [!IMPORTANT]
> hello belastningen generator körs som en serie av jobb i din lokala PowerShell-session. Behåll hello *Demo-PerformanceMonitoringAndManagement.ps1* flik! Om du stänger hello fliken eller pausa din dator, stoppar hello belastningen generator. hello belastningen generator finns kvar i en *jobbet anropar* tillstånd där den genererar belastningen på alla nya klienter som tillhandahålls när hello generator har startats. Använd *Ctrl-C* toostop anropar nya jobb och avsluta hello skript. hello belastningen generator fortsätter toorun, men endast på befintliga klienter.

## <a name="monitor-resource-usage-using-hello-azure-portal"></a>Övervaka Resursanvändning med hello Azure-portalen

toomonitor hello Resursanvändning som resultat från hello läser in tillämpas, öppna hello portal toohello pool som innehåller hello klient databaser:

1. Öppna hello [Azure-portalen](https://portal.azure.com) och bläddra toohello *tenants1 -&lt;användare&gt;*  server.
1. Bläddra ned och hitta elastiska pooler och klicka på **Pool1**. Den här poolen innehåller alla hello klient databaser skapat hittills.

Se hello **elastisk pool övervakning** och **elastisk databas övervakning** diagram.

hello är poolens resursutnyttjandet hello sammanställd databasen användning för alla databaser i hello pool. hello databasen diagrammet visar hello fem senaste databaser:

![](./media/sql-database-saas-tutorial-performance-monitoring/pool1.png)

Eftersom det finns ytterligare databaser i poolen hello utöver Hej fem, hello poolen användning visar aktivitet som inte återspeglas i hello översta fem databaser diagram. Mer information klickar du på **databasen resursutnyttjande**:

![](./media/sql-database-saas-tutorial-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-hello-pool"></a>Ange prestandavarningar i hello-adresspool

Ange en avisering i hello-adresspool som utlösare på \>75% användning på följande sätt:

1. Öppna *Pool1* (på hello *tenants1 -\<användare\>*  server) i hello [Azure-portalen](https://portal.azure.com).
1. Klicka på **aviseringsregler** och därefter på **+ lägg till avisering**:

   ![lägg till avisering](media/sql-database-saas-tutorial-performance-monitoring/add-alert.png)

1. Ange ett namn, exempelvis **hög DTU**,
1. Ange hello följande värden:
   * **Mått = eDTU procent**
   * **Villkor = större än**.
   * **Tröskelvärdet = 75**.
   * **Tidsperiod = över hello senaste 30 minuterna**.
1. Lägg till en e-postadress toohello *ytterligare administratören email(s)* och klicka på **OK**.

   ![ange varning](media/sql-database-saas-tutorial-performance-monitoring/alert-rule.png)


## <a name="scale-up-a-busy-pool"></a>Skala upp en upptagen pool

Om hello sammanställd Belastningsnivå ökar på en pool toohello som maximerar hello poolen och når 100% eDTU-användning, sedan påverkas enskilda databasens prestanda, potentiellt saktas frågan svarstider för alla databaser i hello pool.

**Kort sikt**, Överväg att skala upp hello tooprovide ytterligare resurser eller ta bort databaser från poolen hello (flyttas tooother pooler, eller från hello tooa fristående pooltjänstnivå).

**Längre sikt**, optimera frågor eller index användning tooimprove databasens prestanda. Beroende på hello utfärdar programmets känslighet tooperformance dess bästa praxis tooscale poolen in innan den når 100% eDTU-användning. Använd en avisering toowarn du i förväg.

Du kan simulera poolen upptagen av ökande hello belastning som produceras av hello generator. Orsaka hello databaser tooburst oftare och för längre, stigande hello sammanställd i hello-pool utan att ändra hello krav hello enskilda databaser. Skala upp hello poolen görs enkelt i hello-portalen eller från PowerShell. Den här övningen använder hello-portalen.

1. Ange *$DemoScenario* = **3**, _generera belastningen med längre och oftare belastning per databas_ tooincrease hello intensiteten hello sammanställd belastningen på hello-pool utan att ändra hello belastning som krävs för varje databas.
1. Tryck på **F5** tooapply en belastningen tooall klient-databaser.

1. Gå för**Pool1** i hello Azure-portalen.

Övervakaren hello ökade pool-eDTU-användning i hello övre diagram. Det tar några minuter för hello nya högre belastning tookick, men bör du snabbt se hello poolen starta toohit max användning och som hello belastningen steadies till hello nytt mönster, overloads snabbt hello poolen.

1. tooscale in hello poolen, klickar du på **konfigurera pool** hello överst i hello **Pool1** sidan.
1. Justera hello **Pool-eDTU** inställning för**100**. Hello pool-eDTU ändrar inte inställningar för hello per databas (vilket är fortfarande 50 eDTU max per databas). Du kan se hello per databas inställningar på hello höger sida av hello **konfigurera pool** sidan.
1. Klicka på **spara** toosubmit hello begäran tooscale hello poolen.

Gå tillbaka för**Pool1** > **översikt** tooview hello övervakning diagram. Övervaka hello effekten av att tillhandahålla hello-pool med mer resurser (men med några databaser och en slumpmässig belastning inte är det alltid enkelt toosee säkerhet förrän du kör under en viss tid). När du tittar på hello diagram försedda med i åtanke att 100% på hello övre diagram nu representerar 100 edtu: er, på hello lägre diagram 100% är fortfarande 50 edtu: er som hello per databas max är fortfarande 50 edtu: er.

Databaser förblir online och fullständigt tillgängliga i hela hello-processen. Vid hello bryts sista stund eftersom varje databas är klar toobe aktiverad med hello ny pool-eDTU alla aktiva anslutningar. Programkod bör alltid skrivas tooretry bort anslutningar och så kommer återansluta toohello databasen i hello uppskalade pool.

## <a name="load-balance-between-pools"></a>Belastningsutjämna mellan pooler

Som ett alternativ tooscaling in hello poolen, skapa en pool med andra och flytta databaser till den belastningen toobalance hello mellan hello två pooler. den nya poolen hello måste skapas på toodo hello samma server som hello först.

1. I hello [Azure-portalen](https://portal.azure.com)öppnar hello **tenants1 -&lt;användare&gt;**  server.
1. Klicka på **+ ny pool** toocreate poolen på hello aktuella servern.
1. På hello **elastisk databaspool** mallen:

    1. Ange **namn** för*Pool2*.
    1. Lämna hello prisnivån som **Standardpool**.
    1. Klicka på **konfigurera poolen**,
    1. Ange **Pool-eDTU** för*50 eDTU*.
    1. Klicka på **lägga till databaser** toosee en lista över databaser på hello-server som kan läggas till för*Pool2*.
    1. Välj alla 10 databaser toomove dessa toohello ny pool och klicka sedan på **Välj**. Om du har kört hello belastningen generator, vet hello tjänsten redan att profilen prestanda kräver en pool som är större än hello standardstorlek 50 eDTU och rekommenderar att du börjar med en 100 eDTU-inställning.

    ![Rekommendation](media/sql-database-saas-tutorial-performance-monitoring/configure-pool.png)

    1. Lämna hello standardvärdet på 50 edtu: er för den här självstudien och klicka på **Välj** igen.
    1. Välj **OK** toocreate hello ny pool och toomove hello markerade databaser till den.

Skapa hello poolen och flytta hello databaser tar några minuter. När databaser flyttas de fortfarande är online och fullständigt tillgängliga förrän hello allra senaste tillfället, då alla öppna anslutningar stängs. Så länge som du har några logik för omprövning, ansluter klienter sedan toohello databasen i hello ny pool.

Bläddra för**Pool2** (på hello *tenants1* server) tooopen hello poolen och övervaka dess prestanda. Om du inte ser det vänta etablering av hello ny pool toocomplete.

Du ser nu att resursanvändningen på *Pool1* har tagits bort och att *Pool2* läses nu på samma sätt.

## <a name="manage-performance-of-a-single-database"></a>Hantera prestanda för en enskild databas

Om en enskild databas i en pool påträffar en varaktigt hög belastning, beroende på hello poolen konfiguration kan den tenderar toodominate hello resurser i hello pool och påverkar andra databaser. Om hello aktiviteten är sannolikt toocontinue under en viss tid, flyttas hello databas tillfälligt utanför hello poolen. Detta gör hello databasen toohave hello extra resurser den behöver och isolerar från hello andra databaser.

Den här övningen simulerar hello effekten av Contoso samklang Hall upplever en hög belastning när biljetter går till försäljning för populära samklang.

1. Öppna hello... \\ *Demo-PerformanceMonitoringAndManagement.ps1* skript.
1. Ställ in **$DemoScenario = 5, generera en normal belastning plus en hög belastning för en enskild klient (cirka 95 DTU).**
1. Ställ in **$SingleTenantDatabaseName = contosoconcerthall**
1. Köra hello skript med hjälp av **F5**.


1. I hello [Azure-portalen](https://portal.azure.com) öppna **Pool1**.
1. Inspektera hello **elastisk pool övervakning** diagram och leta efter hello ökade poolen eDTU-användning. Efter en minut eller två hello högre belastning ska starta tookick i och bör du snabbt se att hello poolen träffar 100% belastning.
1. Inspektera hello **elastisk databas övervakning** visas som visar hello senaste databaser i hello senaste timmen. Hej *contosoconcerthall* databasen snart ska visas som en av hello fem senaste databaser.
1. **Klicka på övervakning av hello elastisk databas** **diagram** och öppnas hello **databasen resursutnyttjande** sida där du kan övervaka någon av hello databaserna. På så sätt kan du isolera hello skärm för hello *contosoconcerthall* databas.
1. Hello lista över databaser, klicka på **contosoconcerthall**.
1. Klicka på **prisnivån (skala dtu: er)** tooopen hello **konfigurera prestanda** sida där du kan ange en fristående prestandanivå för hello-databasen.
1. Klicka på hello **Standard** fliken tooopen hello skalningsalternativ i hello standardnivån.
1. Dra hello **DTU skjutreglaget** tooright tooselect **100** dtu: er. Obs detta motsvarar toohello tjänstmålet **S3**.
1. Klicka på **tillämpa** toomove hello databasen utanför hello poolen och gör den ett *Standard S3* databas.
1. När skalning är Slutför, övervaka hello effekt på hello contosoconcerthall databasen och Pool1 på hello elastisk pool och databas-blad.

När hello hög belastning på hello contosoconcerthall databasen subsides du returnera den toohello pool tooreduce kostnader. Om det är oklart om som sker du kan ange en avisering på hello databas som utlöses när dess DTU-användningen sjunker under hello per databas max i hello poolen. Övrning 5 beskriver hur du flyttar en databas till en pool.

## <a name="other-performance-management-patterns"></a>Övriga prestandahanteringsmönster

**Pre-emptive skalning** i hello övningen ovan där utforskade du hur tooscale en isolerad databas du vet vilken databas toolook för. Om hello hantering av Contoso samklang Hall hade Wingtips hello nära förestående biljett försäljning, ha hello-databasen flyttats utanför hello poolen pre-emptively. I annat fall skulle det förmodligen ha krävt en avisering på hello pool eller hello databasen toospot vad som händer. Du vill inte toolearn om detta från hello andra klienter i hello poolen klagande försämrade prestanda. Och om hello-klient kan förutsäga hur länge de behöver ytterligare resurser som du kan konfigurera en Azure Automation runbook toomove hello databas utanför hello poolen och sedan tillbaka i igen på ett definierat schema.

**Klient självbetjäning skalning** eftersom skalning är en aktivitet som kallas enkelt via hello hanterings-API, kan du enkelt skapa hello möjlighet tooscale klient databaser i klient-riktade programmet och erbjuda som en funktion i SaaS-tjänsten. Exempelvis kan klienter self-administer skala upp och ner kanske länkade direkt tootheir fakturering!

**Skala upp och ner poolen på ett schema toomatch användningsmönster**

Där sammanställd klientanvändning följer förutsägbar användningsmönster, kan du använda Azure Automation tooscale poolen uppåt och nedåt enligt ett schema. Skala exempelvis ned en pool efter 18.00 och upp igen innan 06.00 på veckodagar, när du vet att kraven på resursanvändning går ned.



## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Simulera användning på hello klient databaser genom att köra ett angivet belastningen generator
> * Övervakaren hello klient databaser som de svara toohello ökad belastning
> * Skala upp hello elastiska poolen i svaret toohello ökad databasen belastning
> * Etablera en andra elastisk pool tooload saldo hello databas-aktivitet

[Guiden återställ en enskild klient](sql-database-saas-tutorial-restore-single-tenant.md)


## <a name="additional-resources"></a>Ytterligare resurser

* Ytterligare [självstudier som bygger på hello Wingtip SaaS-programdistribution](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastiska SQL-pooler](sql-database-elastic-pool.md)
* [Azure Automation](../automation/automation-intro.md)
* [Log Analytics](sql-database-saas-tutorial-log-analytics.md) - Guide för att konfigurera och använda Log Analytics
