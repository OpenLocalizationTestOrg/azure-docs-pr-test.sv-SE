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
# <a name="azure-sql-database-query-performance-insight"></a><span data-ttu-id="bb485-103">Azure SQL Database Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="bb485-103">Azure SQL Database Query Performance Insight</span></span>
<span data-ttu-id="bb485-104">Hantera och finjustera hello prestanda hos relationsdatabaser är en utmaning som kräver betydande resurser och tid investeringar.</span><span class="sxs-lookup"><span data-stu-id="bb485-104">Managing and tuning hello performance of relational databases is a challenging task that requires significant expertise and time investment.</span></span> <span data-ttu-id="bb485-105">Query Performance Insight kan du toospend mindre tid felsökning databasens prestanda genom att tillhandahålla hello följande:</span><span class="sxs-lookup"><span data-stu-id="bb485-105">Query Performance Insight allows you toospend less time troubleshooting database performance by providing hello following:</span></span>

* <span data-ttu-id="bb485-106">Djupare inblick i dina databaser resursförbrukning (DTU).</span><span class="sxs-lookup"><span data-stu-id="bb485-106">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="bb485-107">hello de vanligaste frågorna med CPU/varaktighet/körningar, vilket potentiellt kan anpassas för bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="bb485-107">hello top queries by CPU/Duration/Execution count, which can potentially be tuned for improved performance.</span></span>
* <span data-ttu-id="bb485-108">Hej möjlighet toodrill ned hello detaljer om en fråga, visa text och historik över resursutnyttjandet.</span><span class="sxs-lookup"><span data-stu-id="bb485-108">hello ability toodrill down into hello details of a query, view its text and history of resource utilization.</span></span> 
* <span data-ttu-id="bb485-109">Prestandajustering anteckningar som visar åtgärder som utförs av [SQL Azure Database Advisor](sql-database-advisor.md)</span><span class="sxs-lookup"><span data-stu-id="bb485-109">Performance tuning annotations that show actions performed by [SQL Azure Database Advisor](sql-database-advisor.md)</span></span>  



## <a name="prerequisites"></a><span data-ttu-id="bb485-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bb485-110">Prerequisites</span></span>
* <span data-ttu-id="bb485-111">Query Performance Insight kräver att [Frågearkivet](https://msdn.microsoft.com/library/dn817826.aspx) är aktiv på din databas.</span><span class="sxs-lookup"><span data-stu-id="bb485-111">Query Performance Insight requires that [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) is active on your database.</span></span> <span data-ttu-id="bb485-112">Om Query Store inte körs hello portal efterfrågar tooturn på.</span><span class="sxs-lookup"><span data-stu-id="bb485-112">If Query Store is not running, hello portal prompts you tooturn it on.</span></span>

## <a name="permissions"></a><span data-ttu-id="bb485-113">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="bb485-113">Permissions</span></span>
<span data-ttu-id="bb485-114">hello följande [rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-what-is.md) behörigheter som är nödvändiga toouse Query Performance Insight:</span><span class="sxs-lookup"><span data-stu-id="bb485-114">hello following [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions are required toouse Query Performance Insight:</span></span> 

* <span data-ttu-id="bb485-115">**Läsaren**, **ägare**, **deltagare**, **SQL DB-deltagare**, eller **SQL Server-deltagare** behörighet nödvändiga tooview hello översta resurs förbrukar frågor och diagram.</span><span class="sxs-lookup"><span data-stu-id="bb485-115">**Reader**, **Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required tooview hello top resource consuming queries and charts.</span></span> 
* <span data-ttu-id="bb485-116">**Ägare**, **deltagare**, **SQL DB-deltagare**, eller **SQL Server-deltagare** behörigheter som är nödvändiga tooview frågetexten.</span><span class="sxs-lookup"><span data-stu-id="bb485-116">**Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required tooview query text.</span></span>

## <a name="using-query-performance-insight"></a><span data-ttu-id="bb485-117">Använda Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="bb485-117">Using Query Performance Insight</span></span>
<span data-ttu-id="bb485-118">Query Performance Insight är enkelt toouse:</span><span class="sxs-lookup"><span data-stu-id="bb485-118">Query Performance Insight is easy toouse:</span></span>

* <span data-ttu-id="bb485-119">Öppna [Azure-portalen](https://portal.azure.com/) och hitta databasen som du vill tooexamine.</span><span class="sxs-lookup"><span data-stu-id="bb485-119">Open [Azure portal](https://portal.azure.com/) and find database that you want tooexamine.</span></span> 
  * <span data-ttu-id="bb485-120">Välj ”Query Performance Insight” vänster menyn under support och felsökning.</span><span class="sxs-lookup"><span data-stu-id="bb485-120">From left-hand side menu, under support and troubleshooting, select “Query Performance Insight”.</span></span>
* <span data-ttu-id="bb485-121">Granska hello lista över de vanligaste frågorna för förbrukning av resursen på första hello-fliken.</span><span class="sxs-lookup"><span data-stu-id="bb485-121">On hello first tab, review hello list of top resource-consuming queries.</span></span>
* <span data-ttu-id="bb485-122">Välj en enskild fråga tooview information.</span><span class="sxs-lookup"><span data-stu-id="bb485-122">Select an individual query tooview its details.</span></span>
* <span data-ttu-id="bb485-123">Öppna [SQL Azure Database Advisor](sql-database-advisor.md) och kontrollera om det finns några rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="bb485-123">Open [SQL Azure Database Advisor](sql-database-advisor.md) and check if any recommendations are available.</span></span>
* <span data-ttu-id="bb485-124">Använd skjutreglagen eller Zooma ikoner toochange observerats intervall.</span><span class="sxs-lookup"><span data-stu-id="bb485-124">Use sliders or zoom icons toochange observed interval.</span></span>
  
    ![instrumentpanel för prestanda](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> <span data-ttu-id="bb485-126">Ett par timmar av data måste toobe som avbildas av Frågearkivet för SQL-databas tooprovide insikter i frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="bb485-126">A couple hours of data needs toobe captured by Query Store for SQL Database tooprovide query performance insights.</span></span> <span data-ttu-id="bb485-127">Om hello databasen har ingen aktivitet eller Query Store inte var aktiv under en viss tidsperiod, är hello diagram tom när du visar den tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="bb485-127">If hello database has no activity or Query Store was not active during a certain time period, hello charts will be empty when displaying that time period.</span></span> <span data-ttu-id="bb485-128">Du kan aktivera Query Store när som helst om den inte körs.</span><span class="sxs-lookup"><span data-stu-id="bb485-128">You may enable Query Store at any time if it is not running.</span></span>   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a><span data-ttu-id="bb485-129">Granska högsta CPU förbrukningsfrågorna</span><span class="sxs-lookup"><span data-stu-id="bb485-129">Review top CPU consuming queries</span></span>
<span data-ttu-id="bb485-130">I hello [portal](http://portal.azure.com) hello följande:</span><span class="sxs-lookup"><span data-stu-id="bb485-130">In hello [portal](http://portal.azure.com) do hello following:</span></span>

1. <span data-ttu-id="bb485-131">Bläddra tooa SQL-databasen och klicka på **alla inställningar** > **stöd + felsökning** > **fråga performance insight**.</span><span class="sxs-lookup"><span data-stu-id="bb485-131">Browse tooa SQL database and click **All settings** > **Support + Troubleshooting** > **Query performance insight**.</span></span> 
   
    ![Query Performance Insight][1]
   
    <span data-ttu-id="bb485-133">hello de vanligaste frågorna öppnas och hello högsta CPU konsumerande frågor visas.</span><span class="sxs-lookup"><span data-stu-id="bb485-133">hello top queries view opens and hello top CPU consuming queries are listed.</span></span>
2. <span data-ttu-id="bb485-134">Klicka på runt hello diagram för information.</span><span class="sxs-lookup"><span data-stu-id="bb485-134">Click around hello chart for details.</span></span><br><span data-ttu-id="bb485-135">hello översta raden visar den totala DTU % hello-databas medan hello staplar processor som används av hello valt frågor under hello valda intervallet (till exempel om **föregående vecka** väljs varje stapel representerar en dag).</span><span class="sxs-lookup"><span data-stu-id="bb485-135">hello top line shows overall DTU% for hello database, while hello bars show CPU% consumed by hello selected queries during hello selected interval (for example, if **Past week** is selected each bar represents one day).</span></span>
   
    ![de vanligaste frågorna][2]
   
    <span data-ttu-id="bb485-137">hello nedre rutnätet representerar aggregerade information för hello synliga frågor.</span><span class="sxs-lookup"><span data-stu-id="bb485-137">hello bottom grid represents aggregated information for hello visible queries.</span></span>
   
   * <span data-ttu-id="bb485-138">Fråge-ID - Unik identifierare för frågan i databasen.</span><span class="sxs-lookup"><span data-stu-id="bb485-138">Query ID - unique identifier of query inside database.</span></span>
   * <span data-ttu-id="bb485-139">CPU per fråga under synliga intervallet (beroende på aggregeringsfunktionen).</span><span class="sxs-lookup"><span data-stu-id="bb485-139">CPU per query during observable interval (depends on aggregation function).</span></span>
   * <span data-ttu-id="bb485-140">Varaktighet per fråga (beroende på aggregeringsfunktionen).</span><span class="sxs-lookup"><span data-stu-id="bb485-140">Duration per query (depends on aggregation function).</span></span>
   * <span data-ttu-id="bb485-141">Totalt antal körningar för en viss fråga.</span><span class="sxs-lookup"><span data-stu-id="bb485-141">Total number of executions for a particular query.</span></span>
     
     <span data-ttu-id="bb485-142">Markera eller avmarkera enskilda frågor tooinclude eller utesluta hello diagram med hjälp av kryssrutorna.</span><span class="sxs-lookup"><span data-stu-id="bb485-142">Select or clear individual queries tooinclude or exclude them from hello chart using checkboxes.</span></span>
3. <span data-ttu-id="bb485-143">Om dina data blir inaktuella, klickar du på hello **uppdatera** knappen.</span><span class="sxs-lookup"><span data-stu-id="bb485-143">If your data becomes stale, click hello **Refresh** button.</span></span>
4. <span data-ttu-id="bb485-144">Du kan använda reglagen och zoomning knappar toochange observationsintervallet och undersöka toppar: ![inställningar](./media/sql-database-query-performance/zoom.png)</span><span class="sxs-lookup"><span data-stu-id="bb485-144">You can use sliders and zoom buttons toochange observation interval and investigate spikes:  ![settings](./media/sql-database-query-performance/zoom.png)</span></span>
5. <span data-ttu-id="bb485-145">Om du vill att en annan vy, du kan också markera **anpassad** fliken och ange:</span><span class="sxs-lookup"><span data-stu-id="bb485-145">Optionally, if you want a different view, you can select **Custom** tab and set:</span></span>
   
   * <span data-ttu-id="bb485-146">Mått (CPU, varaktighet, körningar)</span><span class="sxs-lookup"><span data-stu-id="bb485-146">Metric (CPU, duration, execution count)</span></span>
   * <span data-ttu-id="bb485-147">Tidsintervall (föregående vecka förra månaden senaste 24 timmar).</span><span class="sxs-lookup"><span data-stu-id="bb485-147">Time interval (Last 24 hours, Past week, Past month).</span></span> 
   * <span data-ttu-id="bb485-148">Antal frågor.</span><span class="sxs-lookup"><span data-stu-id="bb485-148">Number of queries.</span></span>
   * <span data-ttu-id="bb485-149">Aggregeringsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="bb485-149">Aggregation function.</span></span>
     
     ![settings](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a><span data-ttu-id="bb485-151">Visa enskilda Frågedetaljer</span><span class="sxs-lookup"><span data-stu-id="bb485-151">Viewing individual query details</span></span>
<span data-ttu-id="bb485-152">information om tooview fråga:</span><span class="sxs-lookup"><span data-stu-id="bb485-152">tooview query details:</span></span>

1. <span data-ttu-id="bb485-153">Klicka på alla frågor i hello lista över de vanligaste frågorna.</span><span class="sxs-lookup"><span data-stu-id="bb485-153">Click any query in hello list of top queries.</span></span>
   
    ![Information](./media/sql-database-query-performance/details.png)
2. <span data-ttu-id="bb485-155">hello detaljvy öppnar hello frågor körning-förbrukning/varaktighet processorer är uppdelad över tid.</span><span class="sxs-lookup"><span data-stu-id="bb485-155">hello details view opens and hello queries CPU consumption/Duration/Execution count is broken down over time.</span></span>
3. <span data-ttu-id="bb485-156">Klicka på runt hello diagram för information.</span><span class="sxs-lookup"><span data-stu-id="bb485-156">Click around hello chart for details.</span></span>
   
   * <span data-ttu-id="bb485-157">Övre diagrammet visar raden med övergripande databasen DTU % och hello-fälten är processor i procent som används av hello valda frågan.</span><span class="sxs-lookup"><span data-stu-id="bb485-157">Top chart shows line with overall database DTU%, and hello bars are CPU% consumed by hello selected query.</span></span>
   * <span data-ttu-id="bb485-158">Det andra diagrammet visas total varaktighet av hello valda frågan.</span><span class="sxs-lookup"><span data-stu-id="bb485-158">Second chart shows total duration by hello selected query.</span></span>
   * <span data-ttu-id="bb485-159">Längst ned diagrammet visar totalt antal körningar av hello valda frågan.</span><span class="sxs-lookup"><span data-stu-id="bb485-159">Bottom chart shows total number of executions by hello selected query.</span></span>
     
     ![Frågedetaljer][3]
4. <span data-ttu-id="bb485-161">Du kan också använda reglagen, Zooma knappar eller klicka på **inställningar** toocustomize hur fråga data visas eller toopick en annan tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="bb485-161">Optionally, use sliders, zoom buttons or click **Settings** toocustomize how query data is displayed, or toopick a different time period.</span></span>

## <a name="review-top-queries-per-duration"></a><span data-ttu-id="bb485-162">Granska de vanligaste frågorna per varaktighet</span><span class="sxs-lookup"><span data-stu-id="bb485-162">Review top queries per duration</span></span>
<span data-ttu-id="bb485-163">Vi har fört två nya mått som kan hjälpa dig att identifiera potentiella flaskhalsar i hello senaste uppdatering av Query Performance Insight: antalet varaktighet och körningen.</span><span class="sxs-lookup"><span data-stu-id="bb485-163">In hello recent update of Query Performance Insight, we introduced two new metrics that can help you identify potential bottlenecks: duration and execution count.</span></span><br>

<span data-ttu-id="bb485-164">Tidskrävande frågor har hello största risken för att låsa resurser längre, blockerar andra användare och begränsa skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="bb485-164">Long-running queries have hello greatest potential for locking resources longer, blocking other users, and limiting scalability.</span></span> <span data-ttu-id="bb485-165">De är också hello bra kandidater för optimering.</span><span class="sxs-lookup"><span data-stu-id="bb485-165">They are also hello best candidates for optimization.</span></span><br>

<span data-ttu-id="bb485-166">tooidentify tidskrävande frågor:</span><span class="sxs-lookup"><span data-stu-id="bb485-166">tooidentify long running queries:</span></span>

1. <span data-ttu-id="bb485-167">Öppna **anpassad** fliken i Query Performance Insight för den valda databasen</span><span class="sxs-lookup"><span data-stu-id="bb485-167">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="bb485-168">Ändra mått toobe **varaktighet**</span><span class="sxs-lookup"><span data-stu-id="bb485-168">Change metrics toobe **duration**</span></span>
3. <span data-ttu-id="bb485-169">Välj antalet frågor och observationsintervallet</span><span class="sxs-lookup"><span data-stu-id="bb485-169">Select number of queries and observation interval</span></span>
4. <span data-ttu-id="bb485-170">Välj aggregeringsfunktionen</span><span class="sxs-lookup"><span data-stu-id="bb485-170">Select aggregation function</span></span>
   
   * <span data-ttu-id="bb485-171">**Summa** alla frågan körningstid adderar under hela observationsintervallet.</span><span class="sxs-lookup"><span data-stu-id="bb485-171">**Sum** adds up all query execution time during whole observation interval.</span></span>
   * <span data-ttu-id="bb485-172">**Max** hittar frågor som körningstid har högsta på hela observationsintervallet.</span><span class="sxs-lookup"><span data-stu-id="bb485-172">**Max** finds queries which execution time was maximum at whole observation interval.</span></span>
   * <span data-ttu-id="bb485-173">**Genomsnittlig** hittar Genomsnittlig körningstid för alla fråga körningar och visar hello upp utanför dessa medelvärden.</span><span class="sxs-lookup"><span data-stu-id="bb485-173">**Avg** finds average execution time of all query executions and show you hello top out of these averages.</span></span> 
     
     ![frågan varaktighet][4]

## <a name="review-top-queries-per-execution-count"></a><span data-ttu-id="bb485-175">Granska de vanligaste frågorna per körningar</span><span class="sxs-lookup"><span data-stu-id="bb485-175">Review top queries per execution count</span></span>
<span data-ttu-id="bb485-176">Stort antal körningar kan inte påverkar själva databasen och användning av resurser kan vara låg, men övergripande få långsam.</span><span class="sxs-lookup"><span data-stu-id="bb485-176">High number of executions might not be affecting database itself and resources usage can be low, but overall application might get slow.</span></span>

<span data-ttu-id="bb485-177">I vissa fall kan leda mycket hög körningar tooincrease för nätverket sändningar.</span><span class="sxs-lookup"><span data-stu-id="bb485-177">In some cases, very high execution count may lead tooincrease of network round trips.</span></span> <span data-ttu-id="bb485-178">Sändningar påverka prestanda avsevärt.</span><span class="sxs-lookup"><span data-stu-id="bb485-178">Round trips significantly affect performance.</span></span> <span data-ttu-id="bb485-179">De är ämne toonetwork svarstid och toodownstream server svarstid.</span><span class="sxs-lookup"><span data-stu-id="bb485-179">They are subject toonetwork latency and toodownstream server latency.</span></span> 

<span data-ttu-id="bb485-180">Till exempel åt många datadrivna webbplatser kraftigt hello databasen för varje användarbegäran.</span><span class="sxs-lookup"><span data-stu-id="bb485-180">For example, many data-driven Web sites heavily access hello database for every user request.</span></span> <span data-ttu-id="bb485-181">Medan anslutningspoolen hjälper kan hello ökad kan nätverkstrafik och belastningen på hello databasserver påverka prestanda negativt.</span><span class="sxs-lookup"><span data-stu-id="bb485-181">While connection pooling helps, hello increased network traffic and processing load on hello database server can adversely affect performance.</span></span>  <span data-ttu-id="bb485-182">Allmänna råd är tookeep avrunda resor tooan absolut minimum.</span><span class="sxs-lookup"><span data-stu-id="bb485-182">General advice is tookeep round trips tooan absolute minimum.</span></span>

<span data-ttu-id="bb485-183">tooidentify utförs vanliga frågor (”chatty”) frågor:</span><span class="sxs-lookup"><span data-stu-id="bb485-183">tooidentify frequently executed queries (“chatty”) queries:</span></span>

1. <span data-ttu-id="bb485-184">Öppna **anpassad** fliken i Query Performance Insight för den valda databasen</span><span class="sxs-lookup"><span data-stu-id="bb485-184">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="bb485-185">Ändra mått toobe **körningar**</span><span class="sxs-lookup"><span data-stu-id="bb485-185">Change metrics toobe **execution count**</span></span>
3. <span data-ttu-id="bb485-186">Välj antalet frågor och observationsintervallet</span><span class="sxs-lookup"><span data-stu-id="bb485-186">Select number of queries and observation interval</span></span>
   
    ![antal frågor för körning][5]

## <a name="understanding-performance-tuning-annotations"></a><span data-ttu-id="bb485-188">Förstå prestanda prestandajustering anteckningar</span><span class="sxs-lookup"><span data-stu-id="bb485-188">Understanding performance tuning annotations</span></span>
<span data-ttu-id="bb485-189">När utforska din arbetsbelastning i Query Performance Insight märker du ikoner med lodrät linje ovanpå hello diagram.</span><span class="sxs-lookup"><span data-stu-id="bb485-189">While exploring your workload in Query Performance Insight, you might notice icons with vertical line on top of hello chart.</span></span><br>

<span data-ttu-id="bb485-190">Ikonerna är anteckningar; de representerar prestanda som påverkar åtgärder som utförs av [SQL Azure Database Advisor](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="bb485-190">These icons are annotations; they represent performance affecting actions performed by [SQL Azure Database Advisor](sql-database-advisor.md).</span></span> <span data-ttu-id="bb485-191">Genom att hovra anteckningen hämta grundläggande information om hello åtgärd:</span><span class="sxs-lookup"><span data-stu-id="bb485-191">By hovering annotation, you get basic information about hello action:</span></span>

![kommentar för frågan][6]

<span data-ttu-id="bb485-193">Om du vill tooknow mer eller använda advisor rekommendation på hello ikonen.</span><span class="sxs-lookup"><span data-stu-id="bb485-193">If you want tooknow more or apply advisor recommendation, click hello icon.</span></span> <span data-ttu-id="bb485-194">Information om åtgärden öppnas.</span><span class="sxs-lookup"><span data-stu-id="bb485-194">It will open details of action.</span></span> <span data-ttu-id="bb485-195">Om det finns en aktiv rekommendation som du kan använda den direkt med hjälp av kommandot.</span><span class="sxs-lookup"><span data-stu-id="bb485-195">If it’s an active recommendation you can apply it straight away using command.</span></span>

![anteckningen Frågedetaljer][7]

### <a name="multiple-annotations"></a><span data-ttu-id="bb485-197">Flera kommentarer.</span><span class="sxs-lookup"><span data-stu-id="bb485-197">Multiple annotations.</span></span>
<span data-ttu-id="bb485-198">Det är möjligt att på grund av zoomningsnivån anteckningar som är nära tooeach andra kommer Kom komprimerats till en.</span><span class="sxs-lookup"><span data-stu-id="bb485-198">It’s possible, that because of zoom level, annotations that are close tooeach other will get collapsed into one.</span></span> <span data-ttu-id="bb485-199">Detta kommer representeras av särskild ikon, klickar på den öppna nytt blad när listan över grupperade anteckningar ska visas.</span><span class="sxs-lookup"><span data-stu-id="bb485-199">This will be represented by special icon, clicking it will open new blade where list of grouped annotations will be shown.</span></span>
<span data-ttu-id="bb485-200">Korrelerar frågor och åtgärder för prestandajustering hjälper toobetter förstå din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="bb485-200">Correlating queries and performance tuning actions can help toobetter understand your workload.</span></span> 

## <a name="optimizing-hello-query-store-configuration-for-query-performance-insight"></a><span data-ttu-id="bb485-201">Optimera hello Query Store-konfiguration för Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="bb485-201">Optimizing hello Query Store configuration for Query Performance Insight</span></span>
<span data-ttu-id="bb485-202">Hello följande Query Store meddelanden kan uppstå under din användning av Query Performance Insight:</span><span class="sxs-lookup"><span data-stu-id="bb485-202">During your use of Query Performance Insight, you might encounter hello following Query Store messages:</span></span>

* <span data-ttu-id="bb485-203">”Query Store har inte konfigurerats korrekt på den här databasen.</span><span class="sxs-lookup"><span data-stu-id="bb485-203">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="bb485-204">Klicka här för mer toolearn ”.</span><span class="sxs-lookup"><span data-stu-id="bb485-204">Click here toolearn more."</span></span>
* <span data-ttu-id="bb485-205">”Query Store har inte konfigurerats korrekt på den här databasen.</span><span class="sxs-lookup"><span data-stu-id="bb485-205">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="bb485-206">Klicka här toochange inställningar ”.</span><span class="sxs-lookup"><span data-stu-id="bb485-206">Click here toochange settings."</span></span> 

<span data-ttu-id="bb485-207">Dessa meddelanden visas vanligen när Query Store inte kan toocollect nya data.</span><span class="sxs-lookup"><span data-stu-id="bb485-207">These messages usually appear when Query Store is not able toocollect new data.</span></span> 

<span data-ttu-id="bb485-208">Första fallet händer när Query Store är i skrivskyddat läge och parametrarna har ställts in optimalt.</span><span class="sxs-lookup"><span data-stu-id="bb485-208">First case happens when Query Store is in Read-Only state and parameters are set optimally.</span></span> <span data-ttu-id="bb485-209">Du kan åtgärda detta genom att öka storleken på Query Store eller rensa Query Store.</span><span class="sxs-lookup"><span data-stu-id="bb485-209">You can fix this by increasing size of Query Store or clearing Query Store.</span></span>

![knappen qds][8]

<span data-ttu-id="bb485-211">Andra fallet sker när Frågearkivet är inaktiverat eller parametrar har ställts in optimalt.</span><span class="sxs-lookup"><span data-stu-id="bb485-211">Second case happens when Query Store is Off or parameters aren’t set optimally.</span></span> <br><span data-ttu-id="bb485-212">Du kan ändra hello kvarhållning och avbilda princip och aktivera Query Store genom att köra kommandona nedan eller direkt från portalen:</span><span class="sxs-lookup"><span data-stu-id="bb485-212">You can change hello Retention and Capture policy and enable Query Store by executing commands below or directly from portal:</span></span>

![knappen qds][9]

### <a name="recommended-retention-and-capture-policy"></a><span data-ttu-id="bb485-214">Rekommenderade kvarhållning och avbilda princip</span><span class="sxs-lookup"><span data-stu-id="bb485-214">Recommended retention and capture policy</span></span>
<span data-ttu-id="bb485-215">Det finns två typer av bevarandeprinciper:</span><span class="sxs-lookup"><span data-stu-id="bb485-215">There are two types of retention policies:</span></span>

* <span data-ttu-id="bb485-216">Storleken baseras - om set tooAUTO kommer rensa data automatiskt när nästan maxstorlek uppnås.</span><span class="sxs-lookup"><span data-stu-id="bb485-216">Size based - if set tooAUTO it will clean data automatically when near max size is reached.</span></span>
* <span data-ttu-id="bb485-217">Klockslag - standard som vi kommer att ange fråga too30 dagar, vilket betyder att, om Query Store körs slut på utrymme, tas bort information som är äldre än 30 dagar</span><span class="sxs-lookup"><span data-stu-id="bb485-217">Time based - by default we will set it too30 days, which means, if Query Store will run out of space, it will delete query information older than 30 days</span></span>

<span data-ttu-id="bb485-218">Samla in princip kan anges till:</span><span class="sxs-lookup"><span data-stu-id="bb485-218">Capture policy could be set to:</span></span>

* <span data-ttu-id="bb485-219">**Alla** -samlar in alla frågor.</span><span class="sxs-lookup"><span data-stu-id="bb485-219">**All** - Captures all queries.</span></span>
* <span data-ttu-id="bb485-220">**Automatisk** -ovanligt frågor och frågor med obetydlig kompilera och köra varaktighet ignoreras.</span><span class="sxs-lookup"><span data-stu-id="bb485-220">**Auto** - Infrequent queries and queries with insignificant compile and execution duration are ignored.</span></span> <span data-ttu-id="bb485-221">Tröskelvärde för antal, kompilera och runtime varaktighet för körning av bestäms internt.</span><span class="sxs-lookup"><span data-stu-id="bb485-221">Thresholds for execution count, compile and runtime duration are internally determined.</span></span> <span data-ttu-id="bb485-222">Det här är standardalternativet för hello.</span><span class="sxs-lookup"><span data-stu-id="bb485-222">This is hello default option.</span></span>
* <span data-ttu-id="bb485-223">**Ingen** -Query Store slutar att fånga in nya frågor, men runtime statistik för redan avbildade frågor samlas fortfarande.</span><span class="sxs-lookup"><span data-stu-id="bb485-223">**None** - Query Store stops capturing new queries, however runtime stats for already captured queries are still collected.</span></span>

<span data-ttu-id="bb485-224">Vi rekommenderar att alla principer tooAUTO och rensa princip too30 dagar:</span><span class="sxs-lookup"><span data-stu-id="bb485-224">We recommend setting all policies tooAUTO and clean policy too30 days:</span></span>

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

<span data-ttu-id="bb485-225">Öka storleken på Query Store.</span><span class="sxs-lookup"><span data-stu-id="bb485-225">Increase size of Query Store.</span></span> <span data-ttu-id="bb485-226">Detta kan vara utförs av anslutande tooa databasen och utfärdande följande fråga:</span><span class="sxs-lookup"><span data-stu-id="bb485-226">This could be performed by connecting tooa database and issuing following query:</span></span>

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

<span data-ttu-id="bb485-227">Dessa inställningar tillämpas gör slutligen Query Store att samla in nya frågor, men om du inte vill toowait avmarkerar du Query Store.</span><span class="sxs-lookup"><span data-stu-id="bb485-227">Applying these settings will eventually make Query Store collecting new queries, however if you don’t want toowait you can clear Query Store.</span></span> 

> [!NOTE]
> <span data-ttu-id="bb485-228">Kör följande fråga tar bort alla aktuella informationen i hello Query Store.</span><span class="sxs-lookup"><span data-stu-id="bb485-228">Executing following query will delete all current information in hello Query Store.</span></span> 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a><span data-ttu-id="bb485-229">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="bb485-229">Summary</span></span>
<span data-ttu-id="bb485-230">Query Performance Insight hjälper dig att förstå hello effekten av din fråga arbetsbelastning och hur det relaterar toodatabase resursförbrukning.</span><span class="sxs-lookup"><span data-stu-id="bb485-230">Query Performance Insight helps you understand hello impact of your query workload and how it relates toodatabase resource consumption.</span></span> <span data-ttu-id="bb485-231">Med den här funktionen kommer du lär dig mer om hello viktigaste förbrukningsfrågorna och enkelt identifiera hello de toofix innan de hunnit bli problem.</span><span class="sxs-lookup"><span data-stu-id="bb485-231">With this feature, you will learn about hello top consuming queries, and easily identify hello ones toofix before they become a problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb485-232">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bb485-232">Next steps</span></span>
<span data-ttu-id="bb485-233">Ytterligare rekommendationer om hur du förbättrar hello prestanda för SQL-databasen klickar du på [rekommendationer](sql-database-advisor.md) på hello **Query Performance Insight** bladet.</span><span class="sxs-lookup"><span data-stu-id="bb485-233">For additional recommendations about improving hello performance of your SQL database, click [Recommendations](sql-database-advisor.md) on hello **Query Performance Insight** blade.</span></span>

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

