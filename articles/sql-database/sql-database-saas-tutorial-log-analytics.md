---
title: "aaaUse logganalys med en app för flera innehavare av SQL-databas | Microsoft Docs"
description: "Konfigurera och använda logganalys (OMS) med hello Azure SQL Database Wingtip SaaS exempelapp"
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
ms.author: billgib; sstein
ms.openlocfilehash: fa9085ce3462939e66853faa2a3cd71e0f6c2581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setup-and-use-log-analytics-oms-with-hello-wingtip-saas-app"></a><span data-ttu-id="32a68-104">Konfigurera och använda logganalys (OMS) med hello Wingtip SaaS-app</span><span class="sxs-lookup"><span data-stu-id="32a68-104">Setup and use Log Analytics (OMS) with hello Wingtip SaaS app</span></span>

<span data-ttu-id="32a68-105">I kursen får du konfigurera och använda *logganalys ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* för övervakning av elastiska pooler och databaser.</span><span class="sxs-lookup"><span data-stu-id="32a68-105">In this tutorial, you set up and use *Log Analytics([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* for monitoring elastic pools and databases.</span></span> <span data-ttu-id="32a68-106">Den bygger på hello [prestandaövervakning och hantering av kursen](sql-database-saas-tutorial-performance-monitoring.md), och visar hur toouse *logganalys* tooaugment hello övervakning och avisering enligt hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="32a68-106">It builds on hello [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md), and shows how toouse *Log Analytics* tooaugment hello monitoring and alerting provided in hello Azure portal.</span></span> <span data-ttu-id="32a68-107">Log Analytics är särskilt lämpligt för övervakning och avisering i skala eftersom den stöder hundratals pooler och hundratusentals databaser.</span><span class="sxs-lookup"><span data-stu-id="32a68-107">Log Analytics is particularly suitable for monitoring and alerting at scale because it supports hundreds of pools and hundreds of thousands of databases.</span></span> <span data-ttu-id="32a68-108">Den erbjuder också en enskild övervakningslösning som kan integrera övervakning av olika program och Azure-tjänster över flera Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="32a68-108">It also provides a single monitoring solution, which can integrate monitoring of different applications and Azure services, across multiple Azure subscriptions.</span></span>

<span data-ttu-id="32a68-109">I den här guiden får du lära du dig hur man:</span><span class="sxs-lookup"><span data-stu-id="32a68-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="32a68-110">Installera och konfigurera Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="32a68-110">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="32a68-111">Använd logganalys toomonitor pooler och databaser</span><span class="sxs-lookup"><span data-stu-id="32a68-111">Use Log Analytics toomonitor pools and databases</span></span>

<span data-ttu-id="32a68-112">den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:</span><span class="sxs-lookup"><span data-stu-id="32a68-112">toocomplete this tutorial, make sure hello following prerequisites are completed:</span></span>

* <span data-ttu-id="32a68-113">hello Wingtip SaaS appen har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="32a68-113">hello Wingtip SaaS app is deployed.</span></span> <span data-ttu-id="32a68-114">toodeploy på mindre än fem minuter finns [distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="32a68-114">toodeploy in less than five minutes, see [Deploy and explore hello Wingtip SaaS application](sql-database-saas-tutorial.md)</span></span>
* <span data-ttu-id="32a68-115">Azure PowerShell ska ha installerats.</span><span class="sxs-lookup"><span data-stu-id="32a68-115">Azure PowerShell is installed.</span></span> <span data-ttu-id="32a68-116">Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span><span class="sxs-lookup"><span data-stu-id="32a68-116">For details, see [Getting started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span></span>

<span data-ttu-id="32a68-117">Se hello [prestandaövervakning och hantering av kursen](sql-database-saas-tutorial-performance-monitoring.md) en beskrivning av hello SaaS scenarier och mönster och hur de påverkar hello krav på en övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="32a68-117">See hello [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md) for a discussion of hello SaaS scenarios and patterns, and how they affect hello requirements on a monitoring solution.</span></span>

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a><span data-ttu-id="32a68-118">Övervaka och hantera prestanda med Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="32a68-118">Monitoring and managing performance with Log Analytics (OMS)</span></span>

<span data-ttu-id="32a68-119">För SQL Database, finns övervakning och avisering tillgängligt för databaser och pooler.</span><span class="sxs-lookup"><span data-stu-id="32a68-119">For SQL Database, monitoring and alerting is available on databases and pools.</span></span> <span data-ttu-id="32a68-120">Den här inbyggda övervakning och avisering är resursspecifika och smidig för litet antal resurser, men är mindre väl lämpade toomonitoring större installationer eller för att tillhandahålla en samlad bild över olika resurser och -prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="32a68-120">This built-in monitoring and alerting is resource-specific and convenient for small numbers of resources, but is less well suited toomonitoring large installations or for providing a unified view across different resources and subscriptions.</span></span>

<span data-ttu-id="32a68-121">För scenarion med stora volymer, kan Log Analytics användas.</span><span class="sxs-lookup"><span data-stu-id="32a68-121">For high-volume scenarios Log Analytics can be used.</span></span> <span data-ttu-id="32a68-122">Detta är en separat Azure-tjänst som ger analytics över skickade diagnostikloggar och telemetri som samlats in i en log analytics-arbetsyta, som kan samla in telemetri från många tjänster och att använda tooquery och Ställ in aviseringar.</span><span class="sxs-lookup"><span data-stu-id="32a68-122">This is a separate Azure service that provides analytics over emitted diagnostic logs and telemetry gathered in a log analytics workspace, which can collect telemetry from many services and be used tooquery and set alerts.</span></span> <span data-ttu-id="32a68-123">Log Analytics tillhandahåller ett inbyggt frågespråk och verktyg för datavisualisering som tillåter operationell dataanalys och visualisering.</span><span class="sxs-lookup"><span data-stu-id="32a68-123">Log Analytics provides a built-in query language and data visualization tools allowing operational data analytics and visualization.</span></span> <span data-ttu-id="32a68-124">hello SQL Analytics lösning innehåller flera fördefinierade elastisk pool och databasen övervakning och avisering vyer och frågor och kan du lägga till egna ad hoc-frågor och spara dem efter behov.</span><span class="sxs-lookup"><span data-stu-id="32a68-124">hello SQL Analytics solution provides several pre-defined elastic pool and database monitoring and alerting views and queries, and lets you add your own ad-hoc queries and save these as needed.</span></span> <span data-ttu-id="32a68-125">OMS innehåller också en designer för anpassade vyer.</span><span class="sxs-lookup"><span data-stu-id="32a68-125">OMS also provides a custom view designer.</span></span>

<span data-ttu-id="32a68-126">Loggen arbetsytorna och analytics Analyslösningar kan öppnas i hello Azure-portalen och i OMS.</span><span class="sxs-lookup"><span data-stu-id="32a68-126">Log Analytics workspaces and analytics solutions can be opened both in hello Azure portal and in OMS.</span></span> <span data-ttu-id="32a68-127">hello Azure-portalen hello senare åtkomstpunkt men kan vara bakom hello OMS-portalen i vissa områden.</span><span class="sxs-lookup"><span data-stu-id="32a68-127">hello Azure portal is hello newer access point but may be behind hello OMS portal in some areas.</span></span>

### <a name="start-hello-load-generator-toocreate-data-tooanalyze"></a><span data-ttu-id="32a68-128">Starta hello belastningen generator toocreate data tooanalyze</span><span class="sxs-lookup"><span data-stu-id="32a68-128">Start hello load generator toocreate data tooanalyze</span></span>

1. <span data-ttu-id="32a68-129">Öppna **Demo-PerformanceMonitoringAndManagement.ps1** i hello **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="32a68-129">Open **Demo-PerformanceMonitoringAndManagement.ps1** in hello **PowerShell ISE**.</span></span> <span data-ttu-id="32a68-130">Behåll det här skriptet öppna som du kanske vill toorun flera hello belastningen generation scenarier under den här kursen.</span><span class="sxs-lookup"><span data-stu-id="32a68-130">Keep this script open as you may want toorun several of hello load generation scenarios during this tutorial.</span></span>
1. <span data-ttu-id="32a68-131">Om du har mindre än fem hyresgäster att etablera en grupp med klienter tooprovide en mer intressant övervakning kontext:</span><span class="sxs-lookup"><span data-stu-id="32a68-131">If you have less than five tenants, provision a batch of tenants tooprovide a more interesting monitoring context:</span></span>
   1. <span data-ttu-id="32a68-132">Ställ in **$DemoScenario = 1,** **Etablera en batch med klienter**</span><span class="sxs-lookup"><span data-stu-id="32a68-132">Set **$DemoScenario = 1,** **Provision a batch of tenants**</span></span>
   1. <span data-ttu-id="32a68-133">Tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="32a68-133">Press **F5** toorun hello script.</span></span>

1. <span data-ttu-id="32a68-134">Ställ in **$DemoScenario** = 2, **Generera en belastning av normal intensitet (cirka 40 DTU)**.</span><span class="sxs-lookup"><span data-stu-id="32a68-134">Set **$DemoScenario** = 2, **Generate normal intensity load (approx 40 DTU)**.</span></span>
1. <span data-ttu-id="32a68-135">Tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="32a68-135">Press **F5** toorun hello script.</span></span>

## <a name="get-hello-wingtip-application-scripts"></a><span data-ttu-id="32a68-136">Hämta hello Wingtip programskript</span><span class="sxs-lookup"><span data-stu-id="32a68-136">Get hello Wingtip application scripts</span></span>

<span data-ttu-id="32a68-137">hello Wingtip biljetter skript och programmets källkod är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="32a68-137">hello Wingtip Tickets scripts and application source code are available in hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="32a68-138">Skriptfilerna finns i hello [Learning moduler mappen](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span><span class="sxs-lookup"><span data-stu-id="32a68-138">Script files are located in hello [Learning Modules folder](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span></span> <span data-ttu-id="32a68-139">Hämta hello **Learning moduler** mappen tooyour lokala datorn, underhålla dess mappstrukturen.</span><span class="sxs-lookup"><span data-stu-id="32a68-139">Download hello **Learning Modules** folder tooyour local computer, maintaining its folder structure.</span></span>

## <a name="installing-and-configuring-log-analytics-and-hello-azure-sql-analytics-solution"></a><span data-ttu-id="32a68-140">Installera och konfigurera logganalys och hello Azure SQL Analytics lösning</span><span class="sxs-lookup"><span data-stu-id="32a68-140">Installing and configuring Log Analytics and hello Azure SQL Analytics solution</span></span>

<span data-ttu-id="32a68-141">Log Analytics är en separat tjänst som behöver toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="32a68-141">Log Analytics is a separate service that needs toobe configured.</span></span> <span data-ttu-id="32a68-142">Log Analytics samlar in loggdata och telemetri och mått i arbetsyta för logganalys.</span><span class="sxs-lookup"><span data-stu-id="32a68-142">Log Analytics collects log data and telemetry and metrics in a log analytics workspace.</span></span> <span data-ttu-id="32a68-143">En arbetsyta är en resurs, precis som andra resurser i Azure och måste skapas.</span><span class="sxs-lookup"><span data-stu-id="32a68-143">A workspace is a resource, just like other resources in Azure, and must be created.</span></span> <span data-ttu-id="32a68-144">Medan hello arbetsytan inte behöver toobe som skapats i hello samma resursgrupp som hello-program som den övervakar, detta gör hello bäst.</span><span class="sxs-lookup"><span data-stu-id="32a68-144">While hello workspace doesn’t need toobe created in hello same resource group as hello application(s) it is monitoring, this often makes hello most sense.</span></span> <span data-ttu-id="32a68-145">Detta gör hello arbetsytan toobe enkelt bort hello program genom att ta bort resursgruppen hello i hello fallet hello Wingtip SaaS-appen.</span><span class="sxs-lookup"><span data-stu-id="32a68-145">In hello case of hello Wingtip SaaS app, this enables hello workspace toobe easily deleted with hello application by simply deleting hello resource group.</span></span>

1. <span data-ttu-id="32a68-146">Öppna... \\Learning moduler\\prestandaövervakning och hantering av\\logganalys\\*Demo-LogAnalytics.ps1* i hello **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="32a68-146">Open ...\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\*Demo-LogAnalytics.ps1* in hello **PowerShell ISE**.</span></span>
1. <span data-ttu-id="32a68-147">Tryck på **F5** toorun hello skript.</span><span class="sxs-lookup"><span data-stu-id="32a68-147">Press **F5** toorun hello script.</span></span>

<span data-ttu-id="32a68-148">Nu bör du kunna öppna logganalys i hello Azure-portalen (eller hello OMS-portalen).</span><span class="sxs-lookup"><span data-stu-id="32a68-148">At this point you should be able open Log Analytics in hello Azure portal (or hello OMS portal).</span></span> <span data-ttu-id="32a68-149">Det tar några minuter för telemetri toobe i hello logganalys-arbetsytan och toobecome som är synliga.</span><span class="sxs-lookup"><span data-stu-id="32a68-149">It will take a few minutes for telemetry toobe collected in hello Log Analytics workspace and toobecome visible.</span></span> <span data-ttu-id="32a68-150">hello längre du lämnar hello system samla in data hello mer intressant hello-upplevelse blir.</span><span class="sxs-lookup"><span data-stu-id="32a68-150">hello longer you leave hello system gathering data hello more interesting hello experience will be.</span></span> <span data-ttu-id="32a68-151">Nu är dags toograb dryck - bara kontrollera hello belastningen generator körs fortfarande!</span><span class="sxs-lookup"><span data-stu-id="32a68-151">Now's a good time toograb a beverage - just make sure hello load generator is still running!</span></span>


## <a name="use-log-analytics-and-hello-sql-analytics-solution-toomonitor-pools-and-databases"></a><span data-ttu-id="32a68-152">Använd logganalys och hello SQL Analytics lösning toomonitor pooler och databaser</span><span class="sxs-lookup"><span data-stu-id="32a68-152">Use Log Analytics and hello SQL Analytics solution toomonitor pools and databases</span></span>


<span data-ttu-id="32a68-153">Öppna Log Analytics och hello OMS-portalen toolook på hello telemetri som samlas in för hello databaser och pooler i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="32a68-153">In this exercise, open Log Analytics and hello OMS portal toolook at hello telemetry being gathered for hello databases and pools.</span></span>

1. <span data-ttu-id="32a68-154">Bläddra toohello [Azure-portalen](https://portal.azure.com) och öppna Log Analytics genom att klicka på fler tjänster, och sök sedan efter logganalys:</span><span class="sxs-lookup"><span data-stu-id="32a68-154">Browse toohello [Azure portal](https://portal.azure.com) and open Log Analytics by clicking More services, then search for Log Analytics:</span></span>

   ![öppna log analytics](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. <span data-ttu-id="32a68-156">Välj hello arbetsyta med namnet *wtploganalytics -&lt;användare&gt;*.</span><span class="sxs-lookup"><span data-stu-id="32a68-156">Select hello workspace named *wtploganalytics-&lt;USER&gt;*.</span></span>

1. <span data-ttu-id="32a68-157">Välj **översikt** tooopen hello logganalys lösning i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="32a68-157">Select **Overview** tooopen hello Log Analytics solution in hello Azure portal.</span></span>
   <span data-ttu-id="32a68-158">![översiktslänk](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span><span class="sxs-lookup"><span data-stu-id="32a68-158">![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span></span>

    <span data-ttu-id="32a68-159">**VIKTIGA**: Det kan ta några minuter innan hello lösningen är aktiv.</span><span class="sxs-lookup"><span data-stu-id="32a68-159">**IMPORTANT**: It may take a couple of minutes before hello solution is active.</span></span> <span data-ttu-id="32a68-160">Ha tålamod!</span><span class="sxs-lookup"><span data-stu-id="32a68-160">Be patient!</span></span>

1. <span data-ttu-id="32a68-161">Klicka på hello Azure SQL Analytics panelen tooopen.</span><span class="sxs-lookup"><span data-stu-id="32a68-161">Click on hello Azure SQL Analytics tile tooopen it.</span></span>

    ![översikt](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analys](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. <span data-ttu-id="32a68-164">Hej vyn i hello lösning bladet rullar åt sidan, med en egen rullningslist längst hello (uppdatera hello blad om det behövs).</span><span class="sxs-lookup"><span data-stu-id="32a68-164">hello view in hello solution blade scrolls sideways, with its own scroll bar at hello bottom (refresh hello blade if needed).</span></span>

1. <span data-ttu-id="32a68-165">Utforska hello olika vyer genom att klicka på dem eller enskilda resurser tooopen en nedåt explorer, där du kan använda hello tid-skjutreglaget i hello vänster överkant eller klicka på ett lodrätt menyraden toofocus i på ett smalare tidsintervallet.</span><span class="sxs-lookup"><span data-stu-id="32a68-165">Explore hello various views by clicking on them or on individual resources tooopen a drill-down explorer, where you can use hello time-slider in hello top left or click on a vertical bar toofocus in on a narrower time slice.</span></span> <span data-ttu-id="32a68-166">Med den här vyn kan välja du enskilda databaser och pooler toofocus på specifika resurser:</span><span class="sxs-lookup"><span data-stu-id="32a68-166">With this view, you can select individual databases or pools toofocus on specific resources:</span></span>

    ![diagram](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. <span data-ttu-id="32a68-168">Tillbaka på hello lösning bladet om du bläddrar i toohello längst till höger visas några sparade frågor som du kan klicka på tooopen och utforska.</span><span class="sxs-lookup"><span data-stu-id="32a68-168">Back on hello solution blade, if you scroll toohello far right you will see some saved queries that you can click on tooopen and explore.</span></span> <span data-ttu-id="32a68-169">Du kan experimentera med att ändra dessa och spara intressanta frågor som du skapar, vilka du sedan kan öppna och använda med andra resurser.</span><span class="sxs-lookup"><span data-stu-id="32a68-169">You can experiment with modifying these, and save any interesting queries you produce, which you can then re-open and use with other resources.</span></span>

1. <span data-ttu-id="32a68-170">Välj OMS-portalen tooopen hello lösning det tillbaka på hello logganalys arbetsytan bladet.</span><span class="sxs-lookup"><span data-stu-id="32a68-170">Back on hello Log Analytics workspace blade, select OMS Portal tooopen hello solution there.</span></span>

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. <span data-ttu-id="32a68-172">Du kan konfigurera aviseringar i hello OMS-portalen.</span><span class="sxs-lookup"><span data-stu-id="32a68-172">In hello OMS portal, you can configure alerts.</span></span> <span data-ttu-id="32a68-173">Klicka på hello avisering del av hello databasen DTU vyn.</span><span class="sxs-lookup"><span data-stu-id="32a68-173">Click on hello alert portion of hello database DTU view.</span></span>

1. <span data-ttu-id="32a68-174">I hello loggen Sök ser vy som visas som du ett stapeldiagram av hello mätvärden som representeras.</span><span class="sxs-lookup"><span data-stu-id="32a68-174">In hello Log Search view that appears you will see a bar graph of hello metrics being represented.</span></span>

    ![loggsökning](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. <span data-ttu-id="32a68-176">Om du klickar på avisering i hello verktygsfältet kommer att kunna toosee hello varningskonfigurationen och kan ändra den.</span><span class="sxs-lookup"><span data-stu-id="32a68-176">If you click on Alert in hello toolbar you will be able toosee hello alert configuration and can change it.</span></span>

    ![lägg till aviseringsregel](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

<span data-ttu-id="32a68-178">hello övervakning och avisering i logganalys och OMS baseras på frågor över hello data hello arbetsytan, till skillnad från hello Varna vid varje resursbladet är resursspecifika.</span><span class="sxs-lookup"><span data-stu-id="32a68-178">hello monitoring and alerting in Log Analytics and OMS is based on queries over hello data in hello workspace, unlike hello alerting on each resource blade, which is resource-specific.</span></span> <span data-ttu-id="32a68-179">Därmed kan du definiera en avisering som söker i alla databaser, istället för att exempelvis definiera en per databas.</span><span class="sxs-lookup"><span data-stu-id="32a68-179">Thus, you can define an alert that looks over all databases, say, rather than defining one per database.</span></span> <span data-ttu-id="32a68-180">Eller skriva en avisering som använder en sammansatt fråga över flera resurstyper.</span><span class="sxs-lookup"><span data-stu-id="32a68-180">Or write an alert that uses a composite query over multiple resource types.</span></span> <span data-ttu-id="32a68-181">Frågor begränsas bara av hello data är tillgängliga i hello arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="32a68-181">Queries are only limited by hello data available in hello workspace.</span></span>

<span data-ttu-id="32a68-182">Log Analytics för SQL-databas debiteras baserat på hello datavolym hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="32a68-182">Log Analytics for SQL Database is charged for based on hello data volume in hello workspace.</span></span> <span data-ttu-id="32a68-183">I kursen får skapat du en kostnadsfri arbetsyta som är begränsad too500MB per dag.</span><span class="sxs-lookup"><span data-stu-id="32a68-183">In this tutorial, you created a Free workspace, which is limited too500MB per day.</span></span> <span data-ttu-id="32a68-184">När gränsen har nåtts läggs inte längre data toohello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="32a68-184">Once that limit is reached data is no longer added toohello workspace.</span></span>


## <a name="next-steps"></a><span data-ttu-id="32a68-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32a68-185">Next steps</span></span>

<span data-ttu-id="32a68-186">I den här guiden lärde du dig hur man:</span><span class="sxs-lookup"><span data-stu-id="32a68-186">In this tutorial you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="32a68-187">Installera och konfigurera Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="32a68-187">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="32a68-188">Använd logganalys toomonitor pooler och databaser</span><span class="sxs-lookup"><span data-stu-id="32a68-188">Use Log Analytics toomonitor pools and databases</span></span>

[<span data-ttu-id="32a68-189">Guide för klientanalys</span><span class="sxs-lookup"><span data-stu-id="32a68-189">Tenant analytics tutorial</span></span>](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a><span data-ttu-id="32a68-190">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="32a68-190">Additional resources</span></span>

* [<span data-ttu-id="32a68-191">Ytterligare självstudier som bygger på hello inledande Wingtip SaaS-programdistribution</span><span class="sxs-lookup"><span data-stu-id="32a68-191">Additional tutorials that build upon hello initial Wingtip SaaS application deployment</span></span>](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [<span data-ttu-id="32a68-192">Azure Log Analytics</span><span class="sxs-lookup"><span data-stu-id="32a68-192">Azure Log Analytics</span></span>](../log-analytics/log-analytics-azure-sql.md)
* [<span data-ttu-id="32a68-193">OMS</span><span class="sxs-lookup"><span data-stu-id="32a68-193">OMS</span></span>](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
