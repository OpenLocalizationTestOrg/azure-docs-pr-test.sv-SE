---
title: "Kör analytics frågor mot Azure SQL-databaser | Microsoft Docs"
description: "Mellan klient analytics-frågor med data som hämtats från flera databaser i Azure SQL Database."
keywords: "SQL-självstudier"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 11/08/2017
ms.author: anjangsh; billgib; genemi
ms.openlocfilehash: 549b6abf5728e50ee365f40326263d391e4b26fd
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/28/2017
---
# <a name="cross-tenant-analytics-using-extracted-data"></a>Mellan klient analytics med hjälp av data som hämtas

I den här kursen går igenom ett scenario med fullständig analytics. Scenariot visar hur analytics kan aktivera företag att fatta smarta beslut. Med data som hämtats från delat databasen kan använda du analytics och få insikter om klient beteende, inklusive deras användning av exempel Wingtip biljetter SaaS-program. Det här scenariot omfattar tre steg: 

1.  **Extrahera data** från varje klient-databas till en butik analytics.
2.  **Optimera extraherade data** för analytics-bearbetning.
3.  Använd **Business Intelligence** verktyg för att rita ut användbar information som hjälper beslutsfattande. 

I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> - Skapa analytics butik om du vill extrahera data till klienten.
> - Använd elastiska jobb för att extrahera data från varje klient-databas till arkivet analytics.
> - Optimera extraherade data (reorganize till en stjärna schemat).
> - Fråga analytics-databas.
> - Använd Power BI för visualisering av data och markera trender i klientdata och ge rekommendation för förbättringar.

![architectureOverView](media/saas-multitenantdb-tenant-analytics/architectureOverview.png)

## <a name="offline-tenant-analytics-pattern"></a>Offline klient analytics mönster

SaaS-program som du utvecklar har åtkomst till en mängd klientdata som lagras i molnet. Data innehåller en omfattande källa för insikter om drift och användning av programmet och beteendet för klienterna. Dessa insights hjälper funktionen utveckling, användbarhet förbättringar och andra investeringar i appen och plattform.

Det är enkelt att komma åt data för alla klienter när alla data i en databas för flera innehavare. Men åtkomst är mer komplexa distribuerade i större skala över tusentals databaser. Ett sätt att Undvik komplexitet är att extrahera data till en analytics-databas eller ett datalager. Du kan sedan fråga i datalagret för att samla in information från biljetter data för alla klienter.

Den här kursen behandlas en fullständig analytics scenario för det här exempelprogrammet för SaaS. Första, elastisk jobb används för att schemalägga extrahering av data från databasen för varje klient. Informationen som skickas till en butik analytics. Arkivet analytics kan antingen vara en SQL-databas eller ett SQL Data Warehouse. För stora data extrahering [Azure Data Factory](../data-factory/introduction.md) är commended.

Därefter sammanställda data förstörs till en uppsättning [star-schema](https://www.wikipedia.org/wiki/Star_schema) tabeller. Tabellerna består av en central faktatabell plus relaterade dimensionstabeller:

- Central faktatabell i stjärnan-schemat innehåller biljett data.
- Dimensionstabellerna innehåller data om handelsplatser, händelser, kunder och köpa datum.

Tillsammans central och tabeller aktivera effektivt analytiska dimensionsbearbetning. Star-schema som används i den här självstudiekursen visas i följande bild:
 
![StarSchema](media/saas-multitenantdb-tenant-analytics/StarSchema.png)

Slutligen tillfrågas tabellerna star-schema. Resultatet av frågan visas visuellt för att fokusera på insikter om klient beteende och deras användning av programmet. Med det här star schemat kan du köra frågor som hjälp att identifiera objekt som liknar följande:

- Vem köpa biljetter och från vilken plats.
- Dolda mönster och trender inom följande områden:
    - Försäljningen av biljetter.
    - Den relativa populariteten för varje plats.

Förstå hur konsekvent varje klient använder tjänsten ger en möjlighet att skapa serviceplaner för att tillgodose deras behov. Den här kursen ger grundläggande exempel på insikter som kan vara uppnår från innehavaren data.

## <a name="setup"></a>Konfiguration

### <a name="prerequisites"></a>Krav

Se till att följande förhandskrav är slutförda för att kunna slutföra den här guiden:

- Databasprogram Wingtip biljetter SaaS flera innehavare distribueras. För att distribuera på mindre än fem minuter finns [distribuera och utforska databasprogram Wingtip biljetter SaaS flera innehavare](saas-multitenantdb-get-started-deploy.md)
- Wingtip SaaS-skript och programmet [källkod](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) laddas ned från GitHub. Se till att *avblockera zip-filen* innan du extraherar innehållet. Kolla in den [allmänna riktlinjer](saas-tenancy-wingtip-app-guidance-tips.md) steg för att ladda ned och avblockera Wingtip biljetter SaaS-skript.
- Power BI Desktop har installerats. [Hämta Power BI Desktop](https://powerbi.microsoft.com/downloads/)
- I gruppen med ytterligare klienter har etablerats, finns det [ **etablera hyresgäster kursen**](saas-multitenantdb-provision-and-catalog.md).
- Ett jobb konto och databas för jobbet har skapats. Se anvisningarna i den [ **schemat management kursen**](saas-multitenantdb-schema-management.md#create-a-job-account-database-and-new-job-account).

### <a name="create-data-for-the-demo"></a>Skapa data för demonstrationen

I den här självstudiekursen utförs analys på försäljningsdata för biljett. I det aktuella steget generera biljett data för alla klienter.  Dessa data extraheras senare för analys. *Se till att du har etablerat i gruppen med klienter som beskrivs ovan, så att du har en beskrivande mängd data*. En tillräckligt stor mängd data kan exponera en mängd olika biljett köp mönster.

1. I **PowerShell ISE**öppnar *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo-TenantAnalytics.ps1*, och anger följande värde:
    - **$DemoScenario** = **1** köpa biljetter för händelser på alla handelsplatser
2. Tryck på **F5** att köra skriptet och skapa biljett köp historik för varje händelse i varje plats.  Skriptet körs under flera minuter att generera tiotusentals biljetter.

### <a name="deploy-the-analytics-store"></a>Distribuera arkivet analytics
Det är ofta många transaktionella delat databaser som tillsammans innehåller alla klientdata. Du måste samla klientdata från delat databasen till en analytics arkivet. Aggregeringen kan effektivt fråga data. I den här kursen är en Azure SQL Database-databas används för att lagra sammanställda data.

I följande steg ska du distribuera analytics store, som kallas **tenantanalytics**. Du kan också distribuera fördefinierade tabeller som fylls senare under kursen:
1. I PowerShell ISE öppnar *...\Learning Modules\Operational Analytics\Tenant Analytics\Demo-TenantAnalytics.ps1* 
2. Ställ in variabeln $DemoScenario i skript för att matcha valet av analytics store. SQL-databas utan columnstore rekommenderas för learning syften.
    - Om du vill använda SQL-databas utan columnstore **$DemoScenario** = **2**
    - Om du vill använda SQL-databas med columnstore **$DemoScenario** = **3**  
3. Tryck på **F5** att köra skriptet demo (som anropar den *distribuera TenantAnalytics<XX>.ps1* skript) som skapar klient analytics store. 

Nu när du har distribuerat programmet och fylls med intressanta klientdata, använda [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) att ansluta **tenants1-huvudmålservern -\<användare\>**  och **katalog-huvudmålservern -\<användare\>**  servrar med inloggningen = *developer*, lösenord =  *P@ssword1* .

![architectureOverView](media/saas-multitenantdb-tenant-analytics/ssmsSignIn.png)

I Object Explorer, utför du följande steg:

1. Expandera den *tenants1-huvudmålservern -\<användare\>*  server.
2. Expandera noden databaser och se *tenants1* databas som innehåller flera innehavare.
3. Expandera den *katalog-huvudmålservern -\<användare\>*  server.
4. Kontrollera att du ser analytics arkivet och jobaccount-databasen.

Se följande databasen i SSMS Object Explorer genom att expandera noden analytics store:

- Tabeller **TicketsRawData** och **EventsRawData** håller extraherade rådata från klient-databaser.
- Tabellerna star-schema är **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, och **dim_Dates** .
- Den **sp_ShredRawExtractedData** lagrad procedur används för att fylla i tabellerna star-schema från rådata tabeller.

![tenantAnalytics](media/saas-multitenantdb-tenant-analytics/tenantAnalytics.png)

## <a name="data-extraction"></a>Extrahering av data 

### <a name="create-target-groups"></a>Skapa målgrupper 

Innan du fortsätter bör du kontrollera att du har distribuerat den konto och jobaccount databasen. I nästa uppsättning steg används elastiska jobb att extrahera data från databasen delat klienter och för att lagra data i arkivet analytics. Sedan det andra jobbet shreds data och lagrar den i tabeller i stjärnan-schemat. Dessa två jobb körs mot två olika målgrupper nämligen **TenantGroup** och **AnalyticsGroup**. Extrahera projektet körs mot TenantGroup som innehåller alla klient-databaser. Förstöring jobbet körs mot AnalyticsGroup som innehåller bara arkivet analytics. Skapa målgrupper med hjälp av följande steg:

1. I SSMS, ansluta till den **jobaccount** databas i katalogen-huvudmålservern -\<användaren\>.
2. Öppna i SSMS, *...\Learning Modules\Operational Analytics\Tenant Analytics\ TargetGroups.sql* 
3. Ändra den @User variabeln längst upp i skriptet, ersätter <User> med användaren värdet används när du har distribuerat databasprogram Wingtip biljetter SaaS flera innehavare.
4. Tryck på **F5** att köra skriptet som skapar två målgrupper.

### <a name="extract-raw-data-from-all-tenants"></a>Extrahera rådata från alla klienter

Transaktioner kan uppstå oftare för *biljetter och kunden* data än för *händelse och platsen* data. Bör du extraherar biljetter och kunden data separat och oftare än du extrahera händelse och platsen data. I det här avsnittet definierar och två separata schemalägga:

- Extrahera biljetter och kundens data.
- Extrahera data om händelser och plats.

Varje jobb extraherar data och publicerar till arkivet analytics. Ett separat jobb shreds det extraherade data till analytics star-schemat.

1. I SSMS, ansluta till den **jobaccount** databas i katalogen-huvudmålservern -\<användaren\> server.
2. Öppna i SSMS, *...\Learning Modules\Operational Analytics\Tenant Analytics\ExtractTickets.sql*.
3. Ändra @User överst i skript och Ersätt <User> med det användarnamn som används när du har distribuerat databasprogram Wingtip biljetter SaaS flera innehavare. 
4. Tryck på **F5** att köra skriptet som skapar och kör jobbet som hämtar biljetter och kunder data från varje klient-databas. Jobbet att spara data i arkivet för analys.
5. Fråga tabellen TicketsRawData i tenantanalytics-databasen så att tabellen fylls med biljetter information från alla klienter.

![ticketExtracts](media/saas-multitenantdb-tenant-analytics/ticketExtracts.png)

Upprepa föregående steg, förutom den här gången Ersätt **\ExtractTickets.sql** med **\ExtractVenuesEvents.sql** i steg 2.

Körs jobbet fyller tabellen EventsRawData i arkivet analytics nya händelser och handelsplatser information från alla klienter. 

## <a name="data-reorganization"></a>Data om

### <a name="shred-extracted-data-to-populate-star-schema-tables"></a>Hjälp extraherade data fylls i stjärnan schemat tabeller

Nästa steg är att bevisa extraherade rådata till en uppsättning tabeller som är optimerade för analytics-frågor. En stjärna-schema används. En central faktatabell innehåller enskilda biljett försäljning poster. Dimensionstabeller fylls med data om handelsplatser, händelser, kunder och köpa datum. 

I det här avsnittet av kursen definierar och kör ett jobb som sammanfogas extraherade rådata med data i tabellerna star-schema. När merge-jobbet är klart rådata tas bort, lämnar tabellerna som är redo att fyllas med nästa klientdata extrahera jobb.

1. I SSMS, ansluta till den **jobaccount** databas i katalogen-huvudmålservern -\<användaren\>.
2. Öppna i SSMS, *...\Learning Modules\Operational Analytics\Tenant Analytics\ShredRawExtractedData.sql*.
3. Tryck på **F5** att köra skriptet för att definiera ett jobb som anropar sp_ShredRawExtractedData lagrad procedur i arkivet analytics.
4. Tillåt tillräckligt med tid för att jobbet ska köras.
    - Kontrollera den **livscykel** kolumn jobs.jobs_execution tabell för status för jobbet. Se till att jobbet **lyckades** innan du fortsätter. En lyckad körning visar data som liknar följande diagram:

![shreddingJob](media/saas-multitenantdb-tenant-analytics/shreddingJob.PNG)

## <a name="data-exploration"></a>Datagranskning

### <a name="visualize-tenant-data"></a>Visualisera klientdata

Data i tabellen star schema innehåller alla biljett säljdata behövs för din analys. Om du vill göra det lättare att se trender i stora datamängder, som du behöver visualisera den grafiskt.  I det här avsnittet får du lära dig hur du använder **Power BI** att ändra och visualisera klientdata du har extraherat och ordnas.

Använd följande steg för att ansluta till Power BI och för att importera de vyer som du skapade tidigare:

1. Starta Power BI desktop.
2. Välj Home-menyfliksområdet **hämta Data**, och välj **mer...** från menyn.
3. I den **hämta Data** fönstret Välj Azure SQL Database.
4. Ange namnet på servern i fönstret databas inloggning (katalog-huvudmålservern -\<användaren\>. database.windows.net). Välj **importera** för **Data anslutningsläget**, och klicka sedan på OK. 

    ![powerBISignIn](media/saas-multitenantdb-tenant-analytics/powerBISignIn.PNG)

5. Välj **databasen** sedan ange användarnamn i det vänstra fönstret = *developer*, och ange lösenord =  *P@ssword1* . Klicka på **Anslut**.  

    ![DatabaseSignIn](media/saas-multitenantdb-tenant-analytics/databaseSignIn.PNG)

6. I den **Navigator** rutan under analytics-databasen, Välj tabellerna som star-schema: fact_Tickets dim_Events, dim_Venues, dim_Customers och dim_Dates. Välj sedan **belastningen**. 

Grattis! Data har lästs in Power BI. Nu kan du börja utforska intressanta visualiseringar för att få insikter om dina klienter. Därefter går igenom hur analytics gör att du kan ge datadrivna rekommendationer för Wingtip biljetter business-teamet. Rekommendationerna som hjälper dig för att optimera business modell och customer experience.

Börja med att analysera biljett försäljningsdata för att se variationen i användning över handelsplatser. Välj följande alternativ i Power BI för att rita ett stapeldiagram av det totala antalet biljetter säljs av varje plats. På grund av slumpmässig variation i biljett generator kan resultaten vara olika.
 
![TotalTicketsByVenues](media/saas-multitenantdb-tenant-analytics/TotalTicketsByVenues.PNG)

Föregående ritytans bekräftar att varierar antalet biljetter säljs av varje plats. Handelsplatser som säljer mer biljetter använder tjänsten tyngre än handelsplatser som säljer färre biljetter. Det kan finnas en möjlighet att skräddarsy resursallokering efter behov för olika innehavare.

Du kan ytterligare analysera data för att se hur Biljettförsäljning variera över tid. Välj följande alternativ i Power BI för att rita det totala antalet biljetter säljs varje dag i 60 dagar.
 
![SaleVersusDate](media/saas-multitenantdb-tenant-analytics/SaleVersusDate.PNG)

Föregående diagram visar att biljetten försäljning topp för vissa handelsplatser. Dessa toppar förstärka idé att vissa handelsplatser kan förbrukar systemresurser oproportionerligt. Hittills är inget uppenbara mönster när toppar inträffar.

Därefter vill undersöka betydelsen av dessa försäljning dagar med hög belastning. När dessa toppar, sker när biljetter går du vidare försäljning? Välj följande alternativ i Power BI om du vill rita biljetter säljs per dag.

![SaleDayDistribution](media/saas-multitenantdb-tenant-analytics/SaleDistributionPerDay.PNG)

Föregående ritytans visar att vissa handelsplatser säljer mycket biljetter på den första dagen i försäljning. Så snart biljetter går du vidare försäljning på dessa handelsplatser, verkar vara en får går lite för snabbt. Den här burst av aktivitet efter några handelsplatser kan det påverka tjänst för andra klienter.

Detaljer om data igen för att se om det här får går lite för snabbt gäller för alla händelser som dessa handelsplatser värd. I tidigare områden sett att Contoso samklang Hall säljer mycket biljetter och att Contoso även har en topp i biljett sales på vissa dagar. Experimentera med Power BI-alternativ för att rita kumulativa Biljettförsäljning för Contoso samklang Hall, fokuserar på försäljning trender för var och en händelse. Följ samma mönster för försäljning i alla händelser?

![ContosoSales](media/saas-multitenantdb-tenant-analytics/EventSaleTrends.PNG)

Föregående området för Contoso samklang Hall visar att den får går lite för snabbt inte sker för alla händelser. Experimentera med filteralternativ att se trender försäljning för mera.

Insikter om biljett sälja mönster leda Wingtip biljetter för att optimera sin affärsmodell. I stället för att ladda alla klienter lika bör kanske Wingtip installera tjänstnivåer med olika prestandanivåer. Större handelsplatser som behöver sälja mer biljetter per dag skulle kunna erbjudas en högre nivå med ett högre servicenivåavtal (SLA). De handelsplatser kan ha sina databaser som placerats i pool med högre gränserna för per databas. Varje tjänstnivå kan ha en timvis allokering, med ytterligare avgifter debiteras för tilldelning. Större handelsplatser som har periodiska belastning av försäljningen skulle dra nytta av de högre nivåerna och Wingtip biljetter kan tjäna pengar på sina service mer effektivt.

Vissa kunder Wingtip biljetter klagar under tiden de behöva kämpa sälja tillräckligt med biljetter om du vill justera kostnaden för tjänsten. Kanske i dessa insikter har möjlighet att öka försäljningen av biljett för under utför handelsplatser. Högre försäljning ökar upplevd värdet för tjänsten. Högerklicka på fact_Tickets och välj **nytt mått**. Ange följande uttryck för det nya måttet som kallas **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Välj följande alternativ för visualisering att rita procentandel biljetterna säljs av varje plats för att fastställa deras relativa resultat.

![analyticsViews](media/saas-multitenantdb-tenant-analytics/AvgTicketsByVenues.PNG)

Föregående ritytans visar att även om de flesta handelsplatser sälja mer än 80% för biljetten, vissa kämpar fylla mer än hälften platser. Experimentera med den värden bra att välja högsta eller lägsta procentandelen biljetter säljs för varje plats.

Tidigare deepened du analyser för att identifiera att Biljettförsäljning tenderar att följa förutsägbar mönster. Den här identifieringen kan Wingtip biljetter hjälp presterar som förväntat handelsplatser öka biljett försäljningen av rekommenderar dynamiska priser. Den här identifieringen kan avslöja möjlighet att använda machine learning-tekniker att förutsäga Biljettförsäljning för varje händelse. Förutsägelser kan även göras för påverkan på intäkter av erbjudandet rabatt på Biljettförsäljning. Power BI Embedded kan integreras i ett program för hantering av händelsen. Integrationen kan hjälpa till att visualisera förväntade försäljning och effekten av olika rabatter. Programmet kan hjälpa dig att utforma en optimal rabatt som ska användas direkt från analytics visningen.

Du har sett trender i klientdata från databasprogram Wingtip biljetter SaaS flera innehavare. Du kan överväga andra sätt appen kan informera affärsbeslut för SaaS-programleverantörer. Leverantörer kan bättre tillgodose behoven för sina klienter. Den här självstudiekursen har förhoppningsvis utrustade du med verktyg som behövs för att utföra analyser på klientdata att möta ditt företag att fatta beslut om datadrivna.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> - Distribuera en klient analytics-databas med fördefinierade stjärnschema tabeller
> - Används för elastiska jobb för att extrahera data från alla klient-databas
> - Sammanfoga data som hämtas till tabeller i en stjärna-schema som utformats för analys
> - Fråga en analytics-databas 
> - Använd Power BI för visualisering av data för att se trender i klientdata 

Grattis!

## <a name="additional-resources"></a>Ytterligare resurser

<!-- - Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials). -->
- [Elastiska jobb](sql-database-elastic-jobs-overview.md).
