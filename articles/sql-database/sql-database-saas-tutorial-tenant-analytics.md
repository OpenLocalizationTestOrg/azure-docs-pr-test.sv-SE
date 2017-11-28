---
title: "aaaRun analytics frågor mot flera Azure SQL-databaser | Microsoft Docs"
description: "Extrahera data från klienten databaser till en analytics-databas för offlineanalys"
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
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.openlocfilehash: f2664e4aafd2fecc98d20d229342bca19b0b08c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a><span data-ttu-id="a95ed-104">Extrahera data från klienten databaser till en analytics-databas för offlineanalys</span><span class="sxs-lookup"><span data-stu-id="a95ed-104">Extract data from tenant databases into an analytics database for offline analysis</span></span>

<span data-ttu-id="a95ed-105">I den här kursen använder du en elastiska jobb toorun frågor mot varje klient-databas.</span><span class="sxs-lookup"><span data-stu-id="a95ed-105">In this tutorial, you use an elastic job toorun queries against each tenant database.</span></span> <span data-ttu-id="a95ed-106">hello jobbet extraherar biljett försäljningsdata och läser in den i en analytics-databas (eller ett datalager) för analys.</span><span class="sxs-lookup"><span data-stu-id="a95ed-106">hello job extracts ticket sales data and loads it into an analytics database (or data warehouse) for analysis.</span></span> <span data-ttu-id="a95ed-107">hello analytics-databas är den efterfrågade tooextract insikter från den här dagliga användningsdata för alla klienter.</span><span class="sxs-lookup"><span data-stu-id="a95ed-107">hello analytics database is then queried tooextract insights from this day-to-day operational data of all tenants.</span></span>


<span data-ttu-id="a95ed-108">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="a95ed-108">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a95ed-109">Skapa hello klient analytics-databas</span><span class="sxs-lookup"><span data-stu-id="a95ed-109">Create hello tenant analytics database</span></span>
> * <span data-ttu-id="a95ed-110">Skapa ett schemalagt jobb tooretrieve data och fylla hello analytics-databas</span><span class="sxs-lookup"><span data-stu-id="a95ed-110">Create a scheduled job tooretrieve data and populate hello analytics database</span></span>

<span data-ttu-id="a95ed-111">toocomplete den här självstudiekursen, se till att hello följande förutsättningar är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="a95ed-111">toocomplete this tutorial, make sure hello following prerequisites are met:</span></span>

* <span data-ttu-id="a95ed-112">hello Wingtip SaaS appen har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="a95ed-112">hello Wingtip SaaS app is deployed.</span></span> <span data-ttu-id="a95ed-113">toodeploy på mindre än fem minuter finns [distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="a95ed-113">toodeploy in less than five minutes, see [Deploy and explore hello Wingtip SaaS application](sql-database-saas-tutorial.md)</span></span>
* <span data-ttu-id="a95ed-114">Azure PowerShell ska ha installerats.</span><span class="sxs-lookup"><span data-stu-id="a95ed-114">Azure PowerShell is installed.</span></span> <span data-ttu-id="a95ed-115">Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span><span class="sxs-lookup"><span data-stu-id="a95ed-115">For details, see [Getting started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span></span>
* <span data-ttu-id="a95ed-116">hello senaste versionen av SQL Server Management Studio (SSMS) är installerad.</span><span class="sxs-lookup"><span data-stu-id="a95ed-116">hello latest version of SQL Server Management Studio (SSMS) is installed.</span></span> [<span data-ttu-id="a95ed-117">Ladda ned och installera SSMS</span><span class="sxs-lookup"><span data-stu-id="a95ed-117">Download and Install SSMS</span></span>](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a><span data-ttu-id="a95ed-118">Klientents användningsanalysmönster</span><span class="sxs-lookup"><span data-stu-id="a95ed-118">Tenant Operational Analytics pattern</span></span>

<span data-ttu-id="a95ed-119">En av hello stora möjligheter med SaaS-program är toouse hello rich-klientdata som lagras i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="a95ed-119">One of hello great opportunities with SaaS applications is toouse hello rich tenant data that is stored in hello cloud.</span></span> <span data-ttu-id="a95ed-120">Använd den här data toogain insikter om hello igen och användning av ditt program och dina klienter.</span><span class="sxs-lookup"><span data-stu-id="a95ed-120">Use this data toogain insights into hello operation and usage of your application, and your tenants.</span></span> <span data-ttu-id="a95ed-121">Den här informationen hjälper funktionen utveckling, användbarhet förbättringar och andra investeringar i hello appen och plattform.</span><span class="sxs-lookup"><span data-stu-id="a95ed-121">This data can guide feature development, usability improvements, and other investments in hello app and platform.</span></span> <span data-ttu-id="a95ed-122">Det är lätt att komma åt denna data när den finns i en enda databas för flera klienter, men inte lika lätt när den är distribuerad i stor skala över potentiellt tusentals databaser.</span><span class="sxs-lookup"><span data-stu-id="a95ed-122">Accessing this data when it's in a single multi-tenant database is easy, but not so easy when distributed at scale across potentially thousands of databases.</span></span> <span data-ttu-id="a95ed-123">En metod tooaccessing dessa data är toouse elastiska jobb, vilket gör resultatet returneras frågeresultaten från jobbet körning toobe i en utdata databas och tabell.</span><span class="sxs-lookup"><span data-stu-id="a95ed-123">One approach tooaccessing this data is toouse Elastic jobs, which enable result-returning query results from job execution toobe captured in an output database and table.</span></span>

## <a name="get-hello-wingtip-application-scripts"></a><span data-ttu-id="a95ed-124">Hämta hello Wingtip programskript</span><span class="sxs-lookup"><span data-stu-id="a95ed-124">Get hello Wingtip application scripts</span></span>

<span data-ttu-id="a95ed-125">hello Wingtip SaaS-skript och programmets källkod är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="a95ed-125">hello Wingtip SaaS scripts and application source code are available in hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="a95ed-126">[Steg toodownload hello Wingtip SaaS skript](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).</span><span class="sxs-lookup"><span data-stu-id="a95ed-126">[Steps toodownload hello Wingtip SaaS scripts](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).</span></span>

## <a name="deploy-a-database-for-tenant-analytics-results"></a><span data-ttu-id="a95ed-127">Distribuera en databas för klientanalysresultat</span><span class="sxs-lookup"><span data-stu-id="a95ed-127">Deploy a database for tenant analytics results</span></span>

<span data-ttu-id="a95ed-128">Den här kursen kräver toohave en databas distribuerade toocapture hello resultat av jobbkörning av skript som innehåller resultatet returneras frågor.</span><span class="sxs-lookup"><span data-stu-id="a95ed-128">This tutorial requires you toohave a database deployed toocapture hello results from job execution of scripts, which contain results-returning queries.</span></span> <span data-ttu-id="a95ed-129">Vi skapar en databas som heter tenantanalytics för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="a95ed-129">Let's create a database called tenantanalytics for this purpose.</span></span>

1. <span data-ttu-id="a95ed-130">Öppna... \\Learning moduler\\operativa Analytics\\klient Analytics\\*Demo-TenantAnalyticsDB.ps1* i hello *PowerShell ISE* och ange Hej följande värde:</span><span class="sxs-lookup"><span data-stu-id="a95ed-130">Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in hello *PowerShell ISE* and set hello following value:</span></span>
   * <span data-ttu-id="a95ed-131">**$DemoScenario** = **2** *distribuera operativ analysdatabas*</span><span class="sxs-lookup"><span data-stu-id="a95ed-131">**$DemoScenario** = **2** *Deploy operational analytics database*</span></span>
1. <span data-ttu-id="a95ed-132">Tryck på **F5** toorun hello demoskript (detta anrop hello *distribuera TenantAnalyticsDB.ps1* skript) som skapar hello klient analytics-databas.</span><span class="sxs-lookup"><span data-stu-id="a95ed-132">Press **F5** toorun hello demo script (that calls hello *Deploy-TenantAnalyticsDB.ps1* script) which creates hello tenant analytics database.</span></span>

## <a name="create-some-data-for-hello-demo"></a><span data-ttu-id="a95ed-133">Skapa vissa data för hello demo</span><span class="sxs-lookup"><span data-stu-id="a95ed-133">Create some data for hello demo</span></span>

1. <span data-ttu-id="a95ed-134">Öppna... \\Learning moduler\\operativa Analytics\\klient Analytics\\*Demo-TenantAnalyticsDB.ps1* i hello *PowerShell ISE* och ange Hej följande värde:</span><span class="sxs-lookup"><span data-stu-id="a95ed-134">Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in hello *PowerShell ISE* and set hello following value:</span></span>
   * <span data-ttu-id="a95ed-135">**$DemoScenario** = **1** *köp biljetter för evenemang på alla platser*</span><span class="sxs-lookup"><span data-stu-id="a95ed-135">**$DemoScenario** = **1** *Purchase tickets for events at all venues*</span></span>
1. <span data-ttu-id="a95ed-136">Tryck på **F5** toorun hello skript och skapa biljett köp historik.</span><span class="sxs-lookup"><span data-stu-id="a95ed-136">Press **F5** toorun hello script and create ticket purchasing history.</span></span>


## <a name="create-a-scheduled-job-tooretrieve-tenant-analytics-about-ticket-purchases"></a><span data-ttu-id="a95ed-137">Skapa ett schemalagt jobb tooretrieve klient analytics om biljett inköp</span><span class="sxs-lookup"><span data-stu-id="a95ed-137">Create a scheduled job tooretrieve tenant analytics about ticket purchases</span></span>

<span data-ttu-id="a95ed-138">Det här skriptet skapar ett jobb tooretrieve biljett Köpinformation från alla klienter.</span><span class="sxs-lookup"><span data-stu-id="a95ed-138">This script creates a job tooretrieve ticket purchase information from all tenants.</span></span> <span data-ttu-id="a95ed-139">När samman till en tabell får du omfattande insiktsfulla mått om biljett köp mönster över hello innehavare.</span><span class="sxs-lookup"><span data-stu-id="a95ed-139">Once aggregated into a single table, you can gain rich insightful metrics about ticket purchasing patterns across hello tenants.</span></span>

1. <span data-ttu-id="a95ed-140">Öppna SSMS och Anslut toohello katalog -&lt;användaren&gt;. database.windows.net server</span><span class="sxs-lookup"><span data-stu-id="a95ed-140">Open SSMS and connect toohello catalog-&lt;user&gt;.database.windows.net server</span></span>
1. <span data-ttu-id="a95ed-141">Öppna... \\inlärningsmoduler\\operativ analys\\klientanalys\\*TicketPurchasesfromAllTenants.sql*</span><span class="sxs-lookup"><span data-stu-id="a95ed-141">Open ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*</span></span>
1. <span data-ttu-id="a95ed-142">Ändra &lt;användare&gt;, Använd hello användarnamnet som används när du distribuerade hello Wingtip SaaS app överst hello hello skriptet **sp\_lägga till\_mål\_grupp\_medlem** och **sp\_lägga till\_jobbsteg**</span><span class="sxs-lookup"><span data-stu-id="a95ed-142">Modify &lt;User&gt;, use hello user name used when you deployed hello Wingtip SaaS app at hello top of hello script, **sp\_add\_target\_group\_member** and **sp\_add\_jobstep**</span></span>
1. <span data-ttu-id="a95ed-143">Högerklicka, Välj **anslutning**, och Anslut toohello katalog -&lt;användaren&gt;. database.windows.net server, om inte redan är ansluten</span><span class="sxs-lookup"><span data-stu-id="a95ed-143">Right click, select **Connection**, and connect toohello catalog-&lt;User&gt;.database.windows.net server, if not already connected</span></span>
1. <span data-ttu-id="a95ed-144">Se till att du är ansluten toohello **jobaccount** databasen och tryck på **F5** att köra hello-skript</span><span class="sxs-lookup"><span data-stu-id="a95ed-144">Ensure you are connected toohello **jobaccount** database and press **F5** to run hello script</span></span>

* <span data-ttu-id="a95ed-145">**SP\_lägga till\_mål\_grupp** skapar hello målgruppnamn *TenantGroup*, nu måste vi tooadd mål medlemmar.</span><span class="sxs-lookup"><span data-stu-id="a95ed-145">**sp\_add\_target\_group** creates hello target group name *TenantGroup*, now we need tooadd target members.</span></span>
* <span data-ttu-id="a95ed-146">**SP\_lägga till\_mål\_grupp\_medlem** lägger till en *server* Medlemstyp som bedömer alla databaser på servern (Obs detta är hello customer1 - mål &lt;Användaren&gt; servern som innehåller hello klient databaser) vid tiden för jobbet körningen ska tas med i hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="a95ed-146">**sp\_add\_target\_group\_member** adds a *server* target member type, which deems all databases within that server (note this is hello customer1-&lt;User&gt; server containing hello tenant databases) at time of job execution should be included in hello job.</span></span>
* <span data-ttu-id="a95ed-147">**sp\_add\_job** skapar ett nytt jobb som schemaläggs veckovis och heter ”biljettköp från alla klienter”</span><span class="sxs-lookup"><span data-stu-id="a95ed-147">**sp\_add\_job** creates a new weekly scheduled job called “Ticket Purchases from all Tenants”</span></span>
* <span data-ttu-id="a95ed-148">**SP\_lägga till\_jobbsteg** skapar hello steg som innehåller T-SQL-kommandot text tooretrieve Köpinformation för alla hello-biljett från alla klienter och kopiera hello returnerar resultat i en tabell med namnet  *AllTicketsPurchasesfromAllTenants*</span><span class="sxs-lookup"><span data-stu-id="a95ed-148">**sp\_add\_jobstep** creates hello job step containing T-SQL command text tooretrieve all hello ticket purchase information from all tenants and copy hello returning result set into a table called *AllTicketsPurchasesfromAllTenants*</span></span>
* <span data-ttu-id="a95ed-149">hello återstående i hello skript i vyerna hello förekomsten av hello objekt och körning av övervakaren jobb.</span><span class="sxs-lookup"><span data-stu-id="a95ed-149">hello remaining views in hello script display hello existence of hello objects and monitor job execution.</span></span> <span data-ttu-id="a95ed-150">Granska hello statusvärde från hello **livscykel** kolumnen toomonitor hello status.</span><span class="sxs-lookup"><span data-stu-id="a95ed-150">Review hello status value from hello **lifecycle** column toomonitor hello status.</span></span> <span data-ttu-id="a95ed-151">En gång, lyckades, hello jobbet har slutförts i alla klient-databaser och hello två nya databaser som innehåller hello till tabellen.</span><span class="sxs-lookup"><span data-stu-id="a95ed-151">Once, Succeeded, hello job has successfully finished on all tenant databases and hello two additional databases containing hello reference table.</span></span>

<span data-ttu-id="a95ed-152">Hello skript körs bör resultera i liknande resultat:</span><span class="sxs-lookup"><span data-stu-id="a95ed-152">Successfully running hello script should result in similar results:</span></span>

![resultat](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-tooretrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a><span data-ttu-id="a95ed-154">Skapa ett jobb tooretrieve sammanfattning antalet biljett inköp från alla klienter</span><span class="sxs-lookup"><span data-stu-id="a95ed-154">Create a job tooretrieve a summary count of ticket purchases from all tenants</span></span>

<span data-ttu-id="a95ed-155">Det här skriptet skapar ett jobb tooretrieve summan av alla biljett inköp från alla klienter.</span><span class="sxs-lookup"><span data-stu-id="a95ed-155">This script creates a job tooretrieve sum of all ticket purchases from all tenants.</span></span>

1. <span data-ttu-id="a95ed-156">Öppna SSMS och Anslut toohello *katalog -&lt;användare&gt;. database.windows.net* server</span><span class="sxs-lookup"><span data-stu-id="a95ed-156">Open SSMS and connect toohello *catalog-&lt;User&gt;.database.windows.net* server</span></span>
1. <span data-ttu-id="a95ed-157">Öppna hello fil... \\Learning moduler\\etablera och katalogen\\operativa Analytics\\klient Analytics\\*TicketPurchasesfromAllTenants.sql resultat*</span><span class="sxs-lookup"><span data-stu-id="a95ed-157">Open hello file …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*</span></span>
1. <span data-ttu-id="a95ed-158">Ändra &lt;användare&gt;, Använd hello användarnamnet som används när du distribuerade hello Wingtip SaaS-app i hello skriptet i hello **sp\_lägga till\_jobbsteg** lagrad procedur</span><span class="sxs-lookup"><span data-stu-id="a95ed-158">Modify &lt;User&gt;, use hello user name used when you deployed hello Wingtip SaaS app in hello script, in hello **sp\_add\_jobstep** stored procedure</span></span>
1. <span data-ttu-id="a95ed-159">Högerklicka, Välj **anslutning**, och Anslut toohello katalog -&lt;användaren&gt;. database.windows.net server, om inte redan är ansluten</span><span class="sxs-lookup"><span data-stu-id="a95ed-159">Right click, select **Connection**, and connect toohello catalog-&lt;User&gt;.database.windows.net server, if not already connected</span></span>
1. <span data-ttu-id="a95ed-160">Se till att du är ansluten toohello **tenantanalytics** databasen och tryck på **F5** att köra hello-skript</span><span class="sxs-lookup"><span data-stu-id="a95ed-160">Ensure you are connected toohello **tenantanalytics** database and press **F5** to run hello script</span></span>

<span data-ttu-id="a95ed-161">Hello skript körs bör resultera i liknande resultat:</span><span class="sxs-lookup"><span data-stu-id="a95ed-161">Successfully running hello script should result in similar results:</span></span>

![resultat](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* <span data-ttu-id="a95ed-163">**sp\_add\_job** skapar en ny schemalagt jobb som heter “ResultsTicketsOrders”</span><span class="sxs-lookup"><span data-stu-id="a95ed-163">**sp\_add\_job** creates a new weekly scheduled job called “ResultsTicketsOrders”</span></span>

* <span data-ttu-id="a95ed-164">**SP\_lägga till\_jobbsteg** skapar hello steg som innehåller T-SQL-kommandot text tooretrieve Köpinformation för alla hello-biljett från alla klienter och kopiera hello returnerar resultat i en tabell som kallas CountofTicketOrders</span><span class="sxs-lookup"><span data-stu-id="a95ed-164">**sp\_add\_jobstep** creates hello job step containing T-SQL command text tooretrieve all hello ticket purchase information from all tenants and copy hello returning result set into a table called CountofTicketOrders</span></span>

* <span data-ttu-id="a95ed-165">hello återstående i hello skript i vyerna hello förekomsten av hello objekt och körning av övervakaren jobb.</span><span class="sxs-lookup"><span data-stu-id="a95ed-165">hello remaining views in hello script display hello existence of hello objects and monitor job execution.</span></span> <span data-ttu-id="a95ed-166">Granska hello statusvärde från hello **livscykel** kolumnen toomonitor hello status.</span><span class="sxs-lookup"><span data-stu-id="a95ed-166">Review hello status value from hello **lifecycle** column toomonitor hello status.</span></span> <span data-ttu-id="a95ed-167">En gång, lyckades, hello jobbet har slutförts i alla klient-databaser och hello två nya databaser som innehåller hello till tabellen.</span><span class="sxs-lookup"><span data-stu-id="a95ed-167">Once, Succeeded, hello job has successfully finished on all tenant databases and hello two additional databases containing hello reference table.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a95ed-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a95ed-168">Next steps</span></span>

<span data-ttu-id="a95ed-169">I den här guiden lärde du dig hur man:</span><span class="sxs-lookup"><span data-stu-id="a95ed-169">In this tutorial you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a95ed-170">Distribuera en klientanalysdatabas</span><span class="sxs-lookup"><span data-stu-id="a95ed-170">Deploy a tenant analytics database</span></span>
> * <span data-ttu-id="a95ed-171">Skapa ett schemalagt jobb tooretrieve analysdata på klienter</span><span class="sxs-lookup"><span data-stu-id="a95ed-171">Create a scheduled job tooretrieve analytical data across tenants</span></span>

<span data-ttu-id="a95ed-172">Grattis!</span><span class="sxs-lookup"><span data-stu-id="a95ed-172">Congratulations!</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a95ed-173">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="a95ed-173">Additional resources</span></span>

* <span data-ttu-id="a95ed-174">Ytterligare [självstudier som bygger på hello Wingtip SaaS-program](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)</span><span class="sxs-lookup"><span data-stu-id="a95ed-174">Additional [tutorials that build upon hello Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)</span></span>
* [<span data-ttu-id="a95ed-175">Elastiska jobb</span><span class="sxs-lookup"><span data-stu-id="a95ed-175">Elastic Jobs</span></span>](sql-database-elastic-jobs-overview.md)
