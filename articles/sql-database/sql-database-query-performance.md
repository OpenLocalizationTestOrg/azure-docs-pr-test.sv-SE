---
title: "aaaQuery insikter i frågeprestanda för Azure SQL Database | Microsoft Docs"
description: "Prestandaövervakning av frågan identifierar hello de flesta CPU-förbrukning av frågor för en Azure SQL Database."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: c2f580b2-3835-453f-89f5-140e02dd2ea7
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 01cca26f85193c679365585cd676449c9db00e1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-query-performance-insight"></a>Azure SQL Database Query Performance Insight
Hantera och finjustera hello prestanda hos relationsdatabaser är en utmaning som kräver betydande resurser och tid investeringar. Query Performance Insight kan du toospend mindre tid felsökning databasens prestanda genom att tillhandahålla hello följande:

* Djupare inblick i dina databaser resursförbrukning (DTU). 
* hello de vanligaste frågorna med CPU/varaktighet/körningar, vilket potentiellt kan anpassas för bättre prestanda.
* Hej möjlighet toodrill ned hello detaljer om en fråga, visa text och historik över resursutnyttjandet. 
* Prestandajustering anteckningar som visar åtgärder som utförs av [SQL Azure Database Advisor](sql-database-advisor.md)  



## <a name="prerequisites"></a>Krav
* Query Performance Insight kräver att [Frågearkivet](https://msdn.microsoft.com/library/dn817826.aspx) är aktiv på din databas. Om Query Store inte körs hello portal efterfrågar tooturn på.

## <a name="permissions"></a>Behörigheter
hello följande [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) behörigheter som är nödvändiga toouse Query Performance Insight: 

* **Läsaren**, **ägare**, **deltagare**, **SQL DB-deltagare**, eller **SQL Server-deltagare** behörighet nödvändiga tooview hello översta resurs förbrukar frågor och diagram. 
* **Ägare**, **deltagare**, **SQL DB-deltagare**, eller **SQL Server-deltagare** behörigheter som är nödvändiga tooview frågetexten.

## <a name="using-query-performance-insight"></a>Använda Query Performance Insight
Query Performance Insight är enkelt toouse:

* Öppna [Azure-portalen](https://portal.azure.com/) och hitta databasen som du vill tooexamine. 
  * Välj ”Query Performance Insight” vänster menyn under support och felsökning.
* Granska hello lista över de vanligaste frågorna för förbrukning av resursen på första hello-fliken.
* Välj en enskild fråga tooview information.
* Öppna [SQL Azure Database Advisor](sql-database-advisor.md) och kontrollera om det finns några rekommendationer.
* Använd skjutreglagen eller Zooma ikoner toochange observerats intervall.
  
    ![instrumentpanel för prestanda](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Ett par timmar av data måste toobe som avbildas av Frågearkivet för SQL-databas tooprovide insikter i frågeprestanda. Om hello databasen har ingen aktivitet eller Query Store inte var aktiv under en viss tidsperiod, är hello diagram tom när du visar den tidsperioden. Du kan aktivera Query Store när som helst om den inte körs.   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a>Granska högsta CPU förbrukningsfrågorna
I hello [portal](http://portal.azure.com) hello följande:

1. Bläddra tooa SQL-databasen och klicka på **alla inställningar** > **stöd + felsökning** > **fråga performance insight**. 
   
    ![Query Performance Insight][1]
   
    hello de vanligaste frågorna öppnas och hello högsta CPU konsumerande frågor visas.
2. Klicka på runt hello diagram för information.<br>hello översta raden visar den totala DTU % hello-databas medan hello staplar processor som används av hello valt frågor under hello valda intervallet (till exempel om **föregående vecka** väljs varje stapel representerar en dag).
   
    ![de vanligaste frågorna][2]
   
    hello nedre rutnätet representerar aggregerade information för hello synliga frågor.
   
   * Fråge-ID - Unik identifierare för frågan i databasen.
   * CPU per fråga under synliga intervallet (beroende på aggregeringsfunktionen).
   * Varaktighet per fråga (beroende på aggregeringsfunktionen).
   * Totalt antal körningar för en viss fråga.
     
     Markera eller avmarkera enskilda frågor tooinclude eller utesluta hello diagram med hjälp av kryssrutorna.
3. Om dina data blir inaktuella, klickar du på hello **uppdatera** knappen.
4. Du kan använda reglagen och zoomning knappar toochange observationsintervallet och undersöka toppar: ![inställningar](./media/sql-database-query-performance/zoom.png)
5. Om du vill att en annan vy, du kan också markera **anpassad** fliken och ange:
   
   * Mått (CPU, varaktighet, körningar)
   * Tidsintervall (föregående vecka förra månaden senaste 24 timmar). 
   * Antal frågor.
   * Aggregeringsfunktionen.
     
     ![settings](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Visa enskilda Frågedetaljer
information om tooview fråga:

1. Klicka på alla frågor i hello lista över de vanligaste frågorna.
   
    ![Information](./media/sql-database-query-performance/details.png)
2. hello detaljvy öppnar hello frågor körning-förbrukning/varaktighet processorer är uppdelad över tid.
3. Klicka på runt hello diagram för information.
   
   * Övre diagrammet visar raden med övergripande databasen DTU % och hello-fälten är processor i procent som används av hello valda frågan.
   * Det andra diagrammet visas total varaktighet av hello valda frågan.
   * Längst ned diagrammet visar totalt antal körningar av hello valda frågan.
     
     ![Frågedetaljer][3]
4. Du kan också använda reglagen, Zooma knappar eller klicka på **inställningar** toocustomize hur fråga data visas eller toopick en annan tidsperiod.

## <a name="review-top-queries-per-duration"></a>Granska de vanligaste frågorna per varaktighet
Vi har fört två nya mått som kan hjälpa dig att identifiera potentiella flaskhalsar i hello senaste uppdatering av Query Performance Insight: antalet varaktighet och körningen.<br>

Tidskrävande frågor har hello största risken för att låsa resurser längre, blockerar andra användare och begränsa skalbarhet. De är också hello bra kandidater för optimering.<br>

tooidentify tidskrävande frågor:

1. Öppna **anpassad** fliken i Query Performance Insight för den valda databasen
2. Ändra mått toobe **varaktighet**
3. Välj antalet frågor och observationsintervallet
4. Välj aggregeringsfunktionen
   
   * **Summa** alla frågan körningstid adderar under hela observationsintervallet.
   * **Max** hittar frågor som körningstid har högsta på hela observationsintervallet.
   * **Genomsnittlig** hittar Genomsnittlig körningstid för alla fråga körningar och visar hello upp utanför dessa medelvärden. 
     
     ![frågan varaktighet][4]

## <a name="review-top-queries-per-execution-count"></a>Granska de vanligaste frågorna per körningar
Stort antal körningar kan inte påverkar själva databasen och användning av resurser kan vara låg, men övergripande få långsam.

I vissa fall kan leda mycket hög körningar tooincrease för nätverket sändningar. Sändningar påverka prestanda avsevärt. De är ämne toonetwork svarstid och toodownstream server svarstid. 

Till exempel åt många datadrivna webbplatser kraftigt hello databasen för varje användarbegäran. Medan anslutningspoolen hjälper kan hello ökad kan nätverkstrafik och belastningen på hello databasserver påverka prestanda negativt.  Allmänna råd är tookeep avrunda resor tooan absolut minimum.

tooidentify utförs vanliga frågor (”chatty”) frågor:

1. Öppna **anpassad** fliken i Query Performance Insight för den valda databasen
2. Ändra mått toobe **körningar**
3. Välj antalet frågor och observationsintervallet
   
    ![antal frågor för körning][5]

## <a name="understanding-performance-tuning-annotations"></a>Förstå prestanda prestandajustering anteckningar
När utforska din arbetsbelastning i Query Performance Insight märker du ikoner med lodrät linje ovanpå hello diagram.<br>

Ikonerna är anteckningar; de representerar prestanda som påverkar åtgärder som utförs av [SQL Azure Database Advisor](sql-database-advisor.md). Genom att hovra anteckningen hämta grundläggande information om hello åtgärd:

![kommentar för frågan][6]

Om du vill tooknow mer eller använda advisor rekommendation på hello ikonen. Information om åtgärden öppnas. Om det finns en aktiv rekommendation som du kan använda den direkt med hjälp av kommandot.

![anteckningen Frågedetaljer][7]

### <a name="multiple-annotations"></a>Flera kommentarer.
Det är möjligt att på grund av zoomningsnivån anteckningar som är nära tooeach andra kommer Kom komprimerats till en. Detta kommer representeras av särskild ikon, klickar på den öppna nytt blad när listan över grupperade anteckningar ska visas.
Korrelerar frågor och åtgärder för prestandajustering hjälper toobetter förstå din arbetsbelastning. 

## <a name="optimizing-hello-query-store-configuration-for-query-performance-insight"></a>Optimera hello Query Store-konfiguration för Query Performance Insight
Hello följande Query Store meddelanden kan uppstå under din användning av Query Performance Insight:

* ”Query Store har inte konfigurerats korrekt på den här databasen. Klicka här för mer toolearn ”.
* ”Query Store har inte konfigurerats korrekt på den här databasen. Klicka här toochange inställningar ”. 

Dessa meddelanden visas vanligen när Query Store inte kan toocollect nya data. 

Första fallet händer när Query Store är i skrivskyddat läge och parametrarna har ställts in optimalt. Du kan åtgärda detta genom att öka storleken på Query Store eller rensa Query Store.

![knappen qds][8]

Andra fallet sker när Frågearkivet är inaktiverat eller parametrar har ställts in optimalt. <br>Du kan ändra hello kvarhållning och avbilda princip och aktivera Query Store genom att köra kommandona nedan eller direkt från portalen:

![knappen qds][9]

### <a name="recommended-retention-and-capture-policy"></a>Rekommenderade kvarhållning och avbilda princip
Det finns två typer av bevarandeprinciper:

* Storleken baseras - om set tooAUTO kommer rensa data automatiskt när nästan maxstorlek uppnås.
* Klockslag - standard som vi kommer att ange fråga too30 dagar, vilket betyder att, om Query Store körs slut på utrymme, tas bort information som är äldre än 30 dagar

Samla in princip kan anges till:

* **Alla** -samlar in alla frågor.
* **Automatisk** -ovanligt frågor och frågor med obetydlig kompilera och köra varaktighet ignoreras. Tröskelvärde för antal, kompilera och runtime varaktighet för körning av bestäms internt. Det här är standardalternativet för hello.
* **Ingen** -Query Store slutar att fånga in nya frågor, men runtime statistik för redan avbildade frågor samlas fortfarande.

Vi rekommenderar att alla principer tooAUTO och rensa princip too30 dagar:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Öka storleken på Query Store. Detta kan vara utförs av anslutande tooa databasen och utfärdande följande fråga:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Dessa inställningar tillämpas gör slutligen Query Store att samla in nya frågor, men om du inte vill toowait avmarkerar du Query Store. 

> [!NOTE]
> Kör följande fråga tar bort alla aktuella informationen i hello Query Store. 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Sammanfattning
Query Performance Insight hjälper dig att förstå hello effekten av din fråga arbetsbelastning och hur det relaterar toodatabase resursförbrukning. Med den här funktionen kommer du lär dig mer om hello viktigaste förbrukningsfrågorna och enkelt identifiera hello de toofix innan de hunnit bli problem.

## <a name="next-steps"></a>Nästa steg
Ytterligare rekommendationer om hur du förbättrar hello prestanda för SQL-databasen klickar du på [rekommendationer](sql-database-advisor.md) på hello **Query Performance Insight** bladet.

![Klassificering av prestanda](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

