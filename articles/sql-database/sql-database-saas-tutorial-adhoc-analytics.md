---
title: "aaaRun ad hoc-analytics frågor över flera Azure SQL-databaser | Microsoft Docs"
description: "Köra ad hoc-frågor för analytics över flera SQL-databaser i hello Wingtip SaaS flera innehavare app."
keywords: sql database tutorial
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: billgib; sstein
ms.openlocfilehash: 140cd51fdd03b5a548147282b51ceb0ad80c944d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-ad-hoc-analytics-queries-across-all-wingtip-saas-tenants"></a>Köra ad hoc-frågor för analytics över alla Wingtip SaaS-klienter

I kursen får köra du distribuerade frågor över hello hela uppsättningen av klient databaser tooenable ad hoc-analytics. Elastisk frågan är används tooenable distribuerade frågor som kräver en databas för ytterligare analys distribueras (toohello katalogserver). Dessa frågor kan extrahera insikter begravd i hello dagliga användningsdata hello Wingtip SaaS-appen.


I den här guiden lär du dig:

> [!div class="checklist"]

> * Om hello globala vyer i varje databas, som gör att effektiva frågor över klienter
> * Hur toodeploy en ad hoc-analytics-databas
> * Hur toorun distribuerade frågor över alla klient-databaser



den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:

* hello Wingtip SaaS appen har distribuerats. toodeploy på mindre än fem minuter finns [distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)
* Azure PowerShell ska ha installerats. Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) har installerats. toodownload och installera SSMS, se [Hämta SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-analytics-pattern"></a>Ad hoc-analytics mönster

En av hello stora möjligheter med SaaS-program är toouse hello mängder med klientdata som lagras centralt i hello molnet. Använd den här data toogain insikter om hello igen och användning av programmet, klienterna, sina användare, inställningar, beteenden och så vidare. Dessa insights hjälper funktionen utveckling, användbarhet förbättringar och andra investeringar i dina appar och tjänster.

Det är lätt att komma åt en enkel databas med flera klienter, men inte så enkelt när du har distribuerat tusentals databaser. En metod som är toouse [elastisk frågan](sql-database-elastic-query-overview.md), vilket innebär att skicka en fråga i en distribuerad uppsättning databaser med gemensamma schemat. Elastisk frågan använder en enda *head* databas i vilken externa tabeller har definierats spegling tabeller eller vyer i hello distribuerade databaser (klient). Frågor som skickats toothis head databasen är kompilerade tooproduce en distribuerad frågeplanen med delar av hello frågan flyttas fram toohello klient databaser efter behov. Elastisk frågan använder hello Fragmentera kartan i hello databasen tooprovide hello katalogplats hello klient databaser. Installation och fråga är enkla standarden [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference), och stöd för ad hoc-frågor från verktyg som Power BI och Excel.

Genom att distribuera frågor över hello klient databaser innehåller elastisk frågan direkta insikter om live produktionsdata. Som elastisk fråga hämtar data från potentiellt många databaser, skickas frågan svarstid kan ibland vara högre än för motsvarande frågor tooa enskilda databaser för flera innehavare. Vara bör försiktig när designa frågar toominimize hello data som returneras. Elastisk frågan är ofta bäst för små mängder data i realtid, frågor som skillnad från toobuilding används ofta komplexa analytics-frågor eller rapporter. Om frågorna inte utför väl kan titta på hello [åtgärdsplan](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) toosee vilken del av hello frågan har aviserats toohello fjärrdatabasen och hur mycket data som returneras. Frågor som kräver komplexa analytisk bearbetning kan vara bättre hanteras i vissa fall genom att extrahera klientdata till en särskild databas eller datalager som är optimerade för analytics-frågor. Det här mönstret förklaras i hello [klient analytics kursen](sql-database-saas-tutorial-tenant-analytics.md). 

## <a name="get-hello-wingtip-application-scripts"></a>Hämta hello Wingtip programskript

hello Wingtip SaaS-skript och programmets källkod är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen. [Steg toodownload hello Wingtip SaaS skript](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-ticket-sales-data"></a>Skapa biljett försäljningsdata

toorun frågor mot en mer intressant datamängd, skapa biljett försäljningsdata genom att köra hello biljett-generatorn.

1. I hello *PowerShell ISE*öppnar hello... \\Learning moduler\\operativa Analytics\\ad hoc Analytics\\*Demo-AdhocAnalytics.ps1* skript och ange hello följande värden:
   * **$DemoScenario** = 1, **köpa biljetter för händelser på alla handelsplatser**.
2. Tryck på **F5** att köra hello skript och generera Biljettförsäljning. När hello skriptet körs, Fortsätt med hello steg i den här självstudiekursen. hello biljett data som efterfrågas i hello *köra ad hoc-distribuerade frågor* avsnittet, så vänta hello biljett generator toocomplete om det fortfarande körs när du får toothat övningen.

## <a name="explore-hello-global-views"></a>Utforska hello globala vyer

hello Wingtip SaaS-programmet skapas med en modell för klient per databas, så hello klient databasschemat definieras ur en enskild klient. Klient-specifik information finns i en tabell *plats*, som alltid har en enda rad och implementeras som en heap utan en primärnyckel. Andra tabeller i hello-schemat inte behöver toobe relaterade toohello *plats* tabellen, eftersom vid normal användning är aldrig osäker vilken klient hello information som hör till.

Men när du frågar över alla databaser är det viktigt att elastisk fråga kan behandla hello data som om det är en del av en enskild logisk databas delat av klient. tooachieve är detta en uppsättning 'globala' vyer tillagda toohello klient databasen som en klient-id till varje hello-tabeller som frågas globalt. Till exempel hello *VenueEvents* visa lägger till en beräknad *VenueId* toohello kolumner planerat från hello *händelser* tabell. Genom att definiera hello extern tabell i hello head databas över *VenueEvents* (i stället för hello underliggande *händelser* tabell), elastisk frågan är kan toopush ned kopplingar baserat på *VenueId* så att de kan köras parallellt på varje fjärrdatabasen (istället för på hello head databas). Detta minskar kraftigt hello mängden data som returneras, vilket resulterar i en stor ökning av prestanda för många frågor. De här globala vyer har skapats i förväg i alla klient-databaser (och i *basetenantdb*).

1. Öppna SSMS och [ansluta toohello tenants1 -&lt;användare&gt; server](sql-database-wtp-overview.md#explore-database-schema-and-execute-sql-queries-using-ssms).
2. Expandera **databaser**, högerklicka på **contosoconcerthall**, och välj **ny fråga**.
3. Kör följande frågor tooexplore hello skillnaden mellan hello stöd för en innehavare tabeller och vyer som hello globala hello:

   ```T-SQL
   -- hello base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice hello plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- hello base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects hello VenueId retrieved from hello Venues table.
   SELECT * FROM VenueEvents
   ```

I dessa vyer hello *VenueId* beräknas eftersom en hash av hello plats namn, men alla metoden skulle kunna använda toointroduce ett unikt värde. Den här metoden är liknande toohello sätt hello klientnyckel beräknas för användning i hello-katalogen.

tooexamine hello definition av hello *handelsplatser* vy:

1. I **Object Explorer**, expandera **contosoconcethall** > **vyer**:

   ![vyer](media/sql-database-saas-tutorial-adhoc-analytics/views.png)

2. Högerklicka på **dbo. Handelsplatser**.
3. Välj **skript som** > **skapa till** > **nya Query Editor-fönstret**

Skript för någon av hello andra *plats* vyer toosee hur de lägger till hello *VenueId*.

## <a name="deploy-hello-database-used-for-ad-hoc-distributed-queries"></a>Distribuera hello-databasen som används för ad hoc-distribuerade frågor

Den här övningen distribuerar hello *adhocanalytics* databas. Detta är head hello-databas som innehåller hello-schema som används för att fråga över alla klient-databaser. hello-databasen är distribuerade toohello befintliga katalogserver, vilket är hello-server som används för alla datorhanteringsrelaterade databaser i hello sample-appen.

1. Öppna... \\Learning moduler\\operativa Analytics\\ad hoc Analytics\\*Demo-AdhocAnalytics.ps1* i hello *PowerShell ISE* och ange hello följande värden:
   * **$DemoScenario** = 2, **Distribuera adhoc-analysdatabas**.

2. Tryck på **F5** toorun hello skript och skapa hello *adhocanalytics* databas.

I nästa avsnitt om hello, du lägger till schemat toohello databasen så att den kan vara används toorun distribuerade frågor.

## <a name="configure-hello-database-for-running-distributed-queries"></a>Konfigurera hello databasen för att köra distribuerade frågor

Den här övningen lägger till schemat (hello extern datakälla och extern tabelldefinitioner) toohello ad hoc-analytics-databas som gör att fråga över alla klient-databaser.

1. Öppna SQL Server Management Studio och Anslut toohello ad hoc Analytics-databas som du skapade i föregående steg i hello. hello namnet på hello databasen kommer att adhocanalytics.
2. Öppna ...\Learning Modules\Operational Analytics\Adhoc Analytics\ *initiera AdhocAnalyticsDB.sql* i SSMS.
3. Granska hello SQL-skript och notera hello följande:

   Elastisk frågan använder en databas-omfattande autentisering tooaccess varje hello klient databaser. Dessa autentiseringsuppgifter måste toobe som är tillgängliga i alla hello-databaser och bör normalt beviljas hello minsta nödvändiga rättigheterna för tooenable dessa ad hoc-frågor.

    ![Skapa autentiseringsuppgifter](media/sql-database-saas-tutorial-adhoc-analytics/create-credential.png)

   hello definierat extern datakälla som är toouse hello klient Fragmentera kartan i hello katalogdatabasen. Med detta som hello extern datakälla kan är frågor distribuerade tooall databaser som är registrerade i hello katalog när hello frågan körs. Eftersom servernamn är olika för varje distribution initieringen skriptet hämtar hello platsen för hello katalogdatabasen genom att hämta aktuella hello-servern (@@servername) där hello skriptet körs.

    ![Skapa extern datakälla](media/sql-database-saas-tutorial-adhoc-analytics/create-external-data-source.png)

   Hej externa tabeller som refererar till hello globala vyer beskrivs i föregående avsnitt i hello och definieras med **DISTRIBUTION = SHARDED(VenueId)**. Eftersom varje *VenueId* mappar tooa enskild databas, detta förbättrar prestanda i många fall enligt hello nästa avsnitt.

    ![Skapa externa tabeller](media/sql-database-saas-tutorial-adhoc-analytics/external-tables.png)

   hello lokal tabell *VenueTypes* som skapas och fylls. Denna referenstabell data är vanligt i alla klient-databaser, så kan representeras som en lokal tabell och ifyllda med hello gemensamma data. För vissa frågor detta kan minska hello mängden data flyttas mellan hello klient databaser och hello *adhocanalytics* databas.

    ![Skapa tabell](media/sql-database-saas-tutorial-adhoc-analytics/create-table.png)

   Om du inkluderar referenstabellerna på detta sätt vara säker på att tooupdate hello-tabellschemat och data när du uppdaterar hello klient databaser.

4. Tryck på **F5** toorun hello skript och initiera hello *adhocanalytics* databas. 

Du kan nu köra distribuerade frågor och samla in insikter över alla klienter!

## <a name="run-ad-hoc-distributed-queries"></a>Köra ad hoc-distribuerade frågor

Nu att hello *adhocanalytics* databasen är ställa in, gå vidare och köra några distribuerade frågor. Inkludera hello åtgärdsplan för bättre förståelse av där hello frågebearbetning sker. 

När undersöks hello åtgärdsplan, hovra över hello plan ikoner för information. 

Viktiga toonote är inställningen **DISTRIBUTION = SHARDED(VenueId)** när vi har definierat hello extern datakälla förbättras prestanda för många scenarier. Eftersom varje *VenueId* mappar tooa enskild databas filtrering är enkelt klart via fjärranslutning, returnerar endast hello data som behövs.

1. Öppna ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* i SSMS.
2. Se till att du är ansluten toohello **adhocanalytics** databas.
3. Välj hello **frågan** -menyn och klicka på **innehåller faktiska körning som planera**
4. Markera hello *vilka handelsplatser är registrerade?* fråga och tryck på **F5**.

   hello frågan returnerar hello hela platsen listan illustrerar hur snabbt och enkelt det är tooquery på alla klienter och returnera data från varje klient.

   Inspektera hello planen och se att hello hela kostnaden hello frågan eftersom vi helt enkelt pågående tooeach klient databasen och välja hello platsen information.

   ![Välj * från dbo. Handelsplatser](media/sql-database-saas-tutorial-adhoc-analytics/query1-plan.png)

5. Välj hello nästa fråga och tryck på **F5**.

   Den här frågan kopplar ihop data från hello klient databaser och hello lokala *VenueTypes* tabellen (lokal, eftersom den är en tabell i hello *adhocanalytics* databas).

   Inspektera hello planen och se den hello merparten av kostnaden är hello frågan eftersom vi söka varje klients plats info (dbo. Handelsplatser) och gör sedan en snabb lokalt join med hello lokala *VenueTypes* toodisplay hello eget namn.

   ![Anslut på fjärrdatorn och den lokala data](media/sql-database-saas-tutorial-adhoc-analytics/query2-plan.png)

6. Nu välja hello *vilken dag har hello de flesta biljetter säljs?* fråga och tryck på **F5**.

   Den här frågan har lite mer komplexa att ansluta och aggregering. Vad är viktigt toonote är att de flesta av hello bearbetningen sker externt och återigen vi hämta tillbaka endast hello rader vi måste returnera en enda rad för varje plats sammanställd biljett Försäljning antal per dag.

   ![DocumentDB](media/sql-database-saas-tutorial-adhoc-analytics/query3-plan.png)


## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]

> * Köra distribuerade frågor över alla klientdatabaser
> * Distribuera en ad hoc-analytics databas och Lägg till schema tooit toorun distribuerade frågor.


Prova nu hello [klient Analytics-självstudier](sql-database-saas-tutorial-tenant-analytics.md) tooexplore extrahera data tooa separat analytics databasen för mer komplexa analyser bearbetning...

## <a name="additional-resources"></a>Ytterligare resurser

* Ytterligare [självstudier som bygger på hello Wingtip SaaS-program](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastic Query](sql-database-elastic-query-overview.md)
