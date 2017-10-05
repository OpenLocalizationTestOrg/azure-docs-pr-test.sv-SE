---
title: "Använda Log Analytics med en SQL Database-app för flera klienter | Microsoft Docs"
description: "Konfigurera och använda logganalys (OMS) med Azure SQL Database Wingtip SaaS exempelapp"
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
ms.openlocfilehash: 26f6f519ecb3abf6343dc2776aa141dff99ced15
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="setup-and-use-log-analytics-oms-with-the-wingtip-saas-app"></a><span data-ttu-id="0dd55-104">Konfigurera och använda logganalys (OMS) med Wingtip SaaS-appen</span><span class="sxs-lookup"><span data-stu-id="0dd55-104">Setup and use Log Analytics (OMS) with the Wingtip SaaS app</span></span>

<span data-ttu-id="0dd55-105">I kursen får du konfigurera och använda *logganalys ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* för övervakning av elastiska pooler och databaser.</span><span class="sxs-lookup"><span data-stu-id="0dd55-105">In this tutorial, you set up and use *Log Analytics([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* for monitoring elastic pools and databases.</span></span> <span data-ttu-id="0dd55-106">Den bygger på [guiden prestandaövervakning och hantering](sql-database-saas-tutorial-performance-monitoring.md) och visar hur du använder *Log Analytics* för att utöka den övervakning och avisering som finns i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0dd55-106">It builds on the [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md), and shows how to use *Log Analytics* to augment the monitoring and alerting provided in the Azure portal.</span></span> <span data-ttu-id="0dd55-107">Log Analytics är särskilt lämpligt för övervakning och avisering i skala eftersom den stöder hundratals pooler och hundratusentals databaser.</span><span class="sxs-lookup"><span data-stu-id="0dd55-107">Log Analytics is particularly suitable for monitoring and alerting at scale because it supports hundreds of pools and hundreds of thousands of databases.</span></span> <span data-ttu-id="0dd55-108">Den erbjuder också en enskild övervakningslösning som kan integrera övervakning av olika program och Azure-tjänster över flera Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="0dd55-108">It also provides a single monitoring solution, which can integrate monitoring of different applications and Azure services, across multiple Azure subscriptions.</span></span>

<span data-ttu-id="0dd55-109">I den här guiden får du lära du dig hur man:</span><span class="sxs-lookup"><span data-stu-id="0dd55-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0dd55-110">Installera och konfigurera Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="0dd55-110">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="0dd55-111">Använda Log Analytics för att övervaka pooler och databaser</span><span class="sxs-lookup"><span data-stu-id="0dd55-111">Use Log Analytics to monitor pools and databases</span></span>

<span data-ttu-id="0dd55-112">Följande krav måste uppfyllas för att kunna köra den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="0dd55-112">To complete this tutorial, make sure the following prerequisites are completed:</span></span>

* <span data-ttu-id="0dd55-113">Wingtip SaaS-appen har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="0dd55-113">The Wingtip SaaS app is deployed.</span></span> <span data-ttu-id="0dd55-114">För att distribuera på mindre än fem minuter finns [distribuera och utforska Wingtip SaaS-program](sql-database-saas-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="0dd55-114">To deploy in less than five minutes, see [Deploy and explore the Wingtip SaaS application](sql-database-saas-tutorial.md)</span></span>
* <span data-ttu-id="0dd55-115">Azure PowerShell ska ha installerats.</span><span class="sxs-lookup"><span data-stu-id="0dd55-115">Azure PowerShell is installed.</span></span> <span data-ttu-id="0dd55-116">Mer information finns i [Kom igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span><span class="sxs-lookup"><span data-stu-id="0dd55-116">For details, see [Getting started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span></span>

<span data-ttu-id="0dd55-117">Se [guiden prestandaövervakning och hantering](sql-database-saas-tutorial-performance-monitoring.md) för en diskussion om SaaS-scenarierna och mönstren och hur de påverkar kraven för en övervakningslösning.</span><span class="sxs-lookup"><span data-stu-id="0dd55-117">See the [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md) for a discussion of the SaaS scenarios and patterns, and how they affect the requirements on a monitoring solution.</span></span>

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a><span data-ttu-id="0dd55-118">Övervaka och hantera prestanda med Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="0dd55-118">Monitoring and managing performance with Log Analytics (OMS)</span></span>

<span data-ttu-id="0dd55-119">För SQL Database, finns övervakning och avisering tillgängligt för databaser och pooler.</span><span class="sxs-lookup"><span data-stu-id="0dd55-119">For SQL Database, monitoring and alerting is available on databases and pools.</span></span> <span data-ttu-id="0dd55-120">Den här inbyggda övervakningen och aviseringen är resursspecifik och praktiskt för små antal resurser, men lämpar sig sämre för övervakning av stora installationer eller för att ge en enhetlig vy över olika resurser och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="0dd55-120">This built-in monitoring and alerting is resource-specific and convenient for small numbers of resources, but is less well suited to monitoring large installations or for providing a unified view across different resources and subscriptions.</span></span>

<span data-ttu-id="0dd55-121">För scenarion med stora volymer, kan Log Analytics användas.</span><span class="sxs-lookup"><span data-stu-id="0dd55-121">For high-volume scenarios Log Analytics can be used.</span></span> <span data-ttu-id="0dd55-122">Det här är en separat Azure-tjänst som tillhandahåller analys för utgivna diagnostikloggar och telemetri som samlats in i en arbetsyta för logganalys. Den samlar telemetri från flera olika tjänster som sedan kan användas för att fråga och ställa in aviseringar.</span><span class="sxs-lookup"><span data-stu-id="0dd55-122">This is a separate Azure service that provides analytics over emitted diagnostic logs and telemetry gathered in a log analytics workspace, which can collect telemetry from many services and be used to query and set alerts.</span></span> <span data-ttu-id="0dd55-123">Log Analytics tillhandahåller ett inbyggt frågespråk och verktyg för datavisualisering som tillåter operationell dataanalys och visualisering.</span><span class="sxs-lookup"><span data-stu-id="0dd55-123">Log Analytics provides a built-in query language and data visualization tools allowing operational data analytics and visualization.</span></span> <span data-ttu-id="0dd55-124">SQL Analytics-lösningen innehåller flera fördefinierade övervaknings- och aviseringsvyer för elastiska pooler och databasövervakning och avisering och frågor. Den låter dig lägga till dina egna ad-hoc-frågor och spara dem efter behov.</span><span class="sxs-lookup"><span data-stu-id="0dd55-124">The SQL Analytics solution provides several pre-defined elastic pool and database monitoring and alerting views and queries, and lets you add your own ad-hoc queries and save these as needed.</span></span> <span data-ttu-id="0dd55-125">OMS innehåller också en designer för anpassade vyer.</span><span class="sxs-lookup"><span data-stu-id="0dd55-125">OMS also provides a custom view designer.</span></span>

<span data-ttu-id="0dd55-126">Log Analytics arbetsytor och analyslösningar kan öppnas både i Azure-portalen och i OMS.</span><span class="sxs-lookup"><span data-stu-id="0dd55-126">Log Analytics workspaces and analytics solutions can be opened both in the Azure portal and in OMS.</span></span> <span data-ttu-id="0dd55-127">Azure-portalen är den nyare åtkomstpunkten, men kan ligga efter OMS-portalen på vissa områden.</span><span class="sxs-lookup"><span data-stu-id="0dd55-127">The Azure portal is the newer access point but may be behind the OMS portal in some areas.</span></span>

### <a name="start-the-load-generator-to-create-data-to-analyze"></a><span data-ttu-id="0dd55-128">Starta belastningsgeneratorn för att skapa data att analysera</span><span class="sxs-lookup"><span data-stu-id="0dd55-128">Start the load generator to create data to analyze</span></span>

1. <span data-ttu-id="0dd55-129">Öppna **Demo-PerformanceMonitoringAndManagement.ps1** i **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="0dd55-129">Open **Demo-PerformanceMonitoringAndManagement.ps1** in the **PowerShell ISE**.</span></span> <span data-ttu-id="0dd55-130">Håll det här skriptet öppet då du kan vilja köra flera scenarion med belastningsgenerering under den här guiden.</span><span class="sxs-lookup"><span data-stu-id="0dd55-130">Keep this script open as you may want to run several of the load generation scenarios during this tutorial.</span></span>
1. <span data-ttu-id="0dd55-131">Om du har mindre än fem klienter, etablerar du en batch med klienter för att skapa en mer intressant övervakningskontext:</span><span class="sxs-lookup"><span data-stu-id="0dd55-131">If you have less than five tenants, provision a batch of tenants to provide a more interesting monitoring context:</span></span>
   1. <span data-ttu-id="0dd55-132">Ställ in **$DemoScenario = 1,** **Etablera en batch med klienter**</span><span class="sxs-lookup"><span data-stu-id="0dd55-132">Set **$DemoScenario = 1,** **Provision a batch of tenants**</span></span>
   1. <span data-ttu-id="0dd55-133">Tryck **F5** för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="0dd55-133">Press **F5** to run the script.</span></span>

1. <span data-ttu-id="0dd55-134">Ställ in **$DemoScenario** = 2, **Generera en belastning av normal intensitet (cirka 40 DTU)**.</span><span class="sxs-lookup"><span data-stu-id="0dd55-134">Set **$DemoScenario** = 2, **Generate normal intensity load (approx 40 DTU)**.</span></span>
1. <span data-ttu-id="0dd55-135">Tryck **F5** för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="0dd55-135">Press **F5** to run the script.</span></span>

## <a name="get-the-wingtip-application-scripts"></a><span data-ttu-id="0dd55-136">Hämta Wingtip-programskripten</span><span class="sxs-lookup"><span data-stu-id="0dd55-136">Get the Wingtip application scripts</span></span>

<span data-ttu-id="0dd55-137">Wingtip biljettskripten och programmets källkod finns tillgängliga från github-repon [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS).</span><span class="sxs-lookup"><span data-stu-id="0dd55-137">The Wingtip Tickets scripts and application source code are available in the [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="0dd55-138">Skriptfilerna finns i [mappen inlärningsmoduler](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span><span class="sxs-lookup"><span data-stu-id="0dd55-138">Script files are located in the [Learning Modules folder](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span></span> <span data-ttu-id="0dd55-139">Ladda ner mappen **inlärningsmoduler** till din lokala dator med sin mappstruktur intakt.</span><span class="sxs-lookup"><span data-stu-id="0dd55-139">Download the **Learning Modules** folder to your local computer, maintaining its folder structure.</span></span>

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a><span data-ttu-id="0dd55-140">Installera och konfigurera Log Analytics och sedan Azure SQL Analytics-lösningen</span><span class="sxs-lookup"><span data-stu-id="0dd55-140">Installing and configuring Log Analytics and the Azure SQL Analytics solution</span></span>

<span data-ttu-id="0dd55-141">Log Analytics är en separat tjänst som måste konfigureras.</span><span class="sxs-lookup"><span data-stu-id="0dd55-141">Log Analytics is a separate service that needs to be configured.</span></span> <span data-ttu-id="0dd55-142">Log Analytics samlar in loggdata och telemetri och mått i arbetsyta för logganalys.</span><span class="sxs-lookup"><span data-stu-id="0dd55-142">Log Analytics collects log data and telemetry and metrics in a log analytics workspace.</span></span> <span data-ttu-id="0dd55-143">En arbetsyta är en resurs, precis som andra resurser i Azure och måste skapas.</span><span class="sxs-lookup"><span data-stu-id="0dd55-143">A workspace is a resource, just like other resources in Azure, and must be created.</span></span> <span data-ttu-id="0dd55-144">Även om arbetsytan inte behöver skapas i samma resursgrupp som programmen den övervakar, känns det ofta mest logiskt.</span><span class="sxs-lookup"><span data-stu-id="0dd55-144">While the workspace doesn’t need to be created in the same resource group as the application(s) it is monitoring, this often makes the most sense.</span></span> <span data-ttu-id="0dd55-145">Detta gör arbetsytan ska raderas enkelt med programmet genom att ta bort resursgruppen för Wingtip SaaS-app.</span><span class="sxs-lookup"><span data-stu-id="0dd55-145">In the case of the Wingtip SaaS app, this enables the workspace to be easily deleted with the application by simply deleting the resource group.</span></span>

1. <span data-ttu-id="0dd55-146">Öppna... \\inlärningsmoduler\\prestandaövervakning och hantering\\Log Analytics\\*Demo-LogAnalytics.ps1* i **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="0dd55-146">Open ...\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\*Demo-LogAnalytics.ps1* in the **PowerShell ISE**.</span></span>
1. <span data-ttu-id="0dd55-147">Tryck **F5** för att köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="0dd55-147">Press **F5** to run the script.</span></span>

<span data-ttu-id="0dd55-148">Nu borde du kunna öppna Log Analytics i Azure-portalen (eller OMS-portalen).</span><span class="sxs-lookup"><span data-stu-id="0dd55-148">At this point you should be able open Log Analytics in the Azure portal (or the OMS portal).</span></span> <span data-ttu-id="0dd55-149">Det tar några minuter för telemetri att samlas in i Log Analytics-arbetsytan och bli synlig.</span><span class="sxs-lookup"><span data-stu-id="0dd55-149">It will take a few minutes for telemetry to be collected in the Log Analytics workspace and to become visible.</span></span> <span data-ttu-id="0dd55-150">Ju längre du lämnar systemet med att samla in data, ju mer intressant blir upplevelsen.</span><span class="sxs-lookup"><span data-stu-id="0dd55-150">The longer you leave the system gathering data the more interesting the experience will be.</span></span> <span data-ttu-id="0dd55-151">Ett bra tillfälle att hämta något att dricka. Se bara till att belastningsgeneratorn är igång!</span><span class="sxs-lookup"><span data-stu-id="0dd55-151">Now's a good time to grab a beverage - just make sure the load generator is still running!</span></span>


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a><span data-ttu-id="0dd55-152">Använd Log Analytics och SQL Analytics-lösningen för att övervaka pooler och databaser</span><span class="sxs-lookup"><span data-stu-id="0dd55-152">Use Log Analytics and the SQL Analytics solution to monitor pools and databases</span></span>


<span data-ttu-id="0dd55-153">Öppna Log Analytics och OMS-portalen för att titta på telemetrin som samlas in för databaser och pooler i den här övningen.</span><span class="sxs-lookup"><span data-stu-id="0dd55-153">In this exercise, open Log Analytics and the OMS portal to look at the telemetry being gathered for the databases and pools.</span></span>

1. <span data-ttu-id="0dd55-154">Bläddra till [Azure-portalen](https://portal.azure.com) och öppna Log Analytics genom att klicka på fler tjänster och därefter söka efter Log Analytics:</span><span class="sxs-lookup"><span data-stu-id="0dd55-154">Browse to the [Azure portal](https://portal.azure.com) and open Log Analytics by clicking More services, then search for Log Analytics:</span></span>

   ![öppna log analytics](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. <span data-ttu-id="0dd55-156">Välj arbetsytan med namnet *wtploganalytics-&lt;användare&gt;*.</span><span class="sxs-lookup"><span data-stu-id="0dd55-156">Select the workspace named *wtploganalytics-&lt;USER&gt;*.</span></span>

1. <span data-ttu-id="0dd55-157">Välj **översikt** för att öppna Log Analytics-lösningen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0dd55-157">Select **Overview** to open the Log Analytics solution in the Azure portal.</span></span>
   <span data-ttu-id="0dd55-158">![översiktslänk](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span><span class="sxs-lookup"><span data-stu-id="0dd55-158">![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span></span>

    <span data-ttu-id="0dd55-159">**Viktigt**: Det kan ta några minuter innan lösningen är aktiv.</span><span class="sxs-lookup"><span data-stu-id="0dd55-159">**IMPORTANT**: It may take a couple of minutes before the solution is active.</span></span> <span data-ttu-id="0dd55-160">Ha tålamod!</span><span class="sxs-lookup"><span data-stu-id="0dd55-160">Be patient!</span></span>

1. <span data-ttu-id="0dd55-161">Klicka på Azure SQL Analytics-ikonen för att öppna den.</span><span class="sxs-lookup"><span data-stu-id="0dd55-161">Click on the Azure SQL Analytics tile to open it.</span></span>

    ![översikt](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analys](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. <span data-ttu-id="0dd55-164">Vyn i lösningens blad rullar åt sidan, med sin egen rullningslist längst ned (uppdatera bladet vid behov).</span><span class="sxs-lookup"><span data-stu-id="0dd55-164">The view in the solution blade scrolls sideways, with its own scroll bar at the bottom (refresh the blade if needed).</span></span>

1. <span data-ttu-id="0dd55-165">Utforska de olika vyerna genom att klicka på dem eller på enskilda resurser för att gå in i detalj där du kan använda tidsreglaget i övre vänstra hörnet, eller klicka på en lodrät stapel för att fokusera på ett snävare tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="0dd55-165">Explore the various views by clicking on them or on individual resources to open a drill-down explorer, where you can use the time-slider in the top left or click on a vertical bar to focus in on a narrower time slice.</span></span> <span data-ttu-id="0dd55-166">Med den här vyn, kan du välja enskilda databaser eller pooler för att fokusera på specifika resurser:</span><span class="sxs-lookup"><span data-stu-id="0dd55-166">With this view, you can select individual databases or pools to focus on specific resources:</span></span>

    ![diagram](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. <span data-ttu-id="0dd55-168">Tillbaka till lösningsbladet, kan du bläddra längst till höger för att se några sparade frågor som du kan klicka på för att öppna och utforska.</span><span class="sxs-lookup"><span data-stu-id="0dd55-168">Back on the solution blade, if you scroll to the far right you will see some saved queries that you can click on to open and explore.</span></span> <span data-ttu-id="0dd55-169">Du kan experimentera med att ändra dessa och spara intressanta frågor som du skapar, vilka du sedan kan öppna och använda med andra resurser.</span><span class="sxs-lookup"><span data-stu-id="0dd55-169">You can experiment with modifying these, and save any interesting queries you produce, which you can then re-open and use with other resources.</span></span>

1. <span data-ttu-id="0dd55-170">Tillbaka i Log Analytics-arbetsytebladet, väljer du OMS-portalen för att öppna lösningen där.</span><span class="sxs-lookup"><span data-stu-id="0dd55-170">Back on the Log Analytics workspace blade, select OMS Portal to open the solution there.</span></span>

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. <span data-ttu-id="0dd55-172">I OMS-portalen kan du konfigurera aviseringar.</span><span class="sxs-lookup"><span data-stu-id="0dd55-172">In the OMS portal, you can configure alerts.</span></span> <span data-ttu-id="0dd55-173">Klicka på aviseringsdelen av databasens DTU-vy.</span><span class="sxs-lookup"><span data-stu-id="0dd55-173">Click on the alert portion of the database DTU view.</span></span>

1. <span data-ttu-id="0dd55-174">I loggsökningsvyn som visas, ser du ett stapeldiagram med de mått som representeras.</span><span class="sxs-lookup"><span data-stu-id="0dd55-174">In the Log Search view that appears you will see a bar graph of the metrics being represented.</span></span>

    ![loggsökning](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. <span data-ttu-id="0dd55-176">Om du klickar på avisering i verktygsfältet, kommer att kunna se aviseringskonfigurationen och ändra den.</span><span class="sxs-lookup"><span data-stu-id="0dd55-176">If you click on Alert in the toolbar you will be able to see the alert configuration and can change it.</span></span>

    ![lägg till aviseringsregel](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

<span data-ttu-id="0dd55-178">Övervakning och aviseringar i Log Analytics och OMS baseras på frågor över data i arbetsytan, till skillnad från aviseringar på varje resursblad, som är resursspecifika.</span><span class="sxs-lookup"><span data-stu-id="0dd55-178">The monitoring and alerting in Log Analytics and OMS is based on queries over the data in the workspace, unlike the alerting on each resource blade, which is resource-specific.</span></span> <span data-ttu-id="0dd55-179">Därmed kan du definiera en avisering som söker i alla databaser, istället för att exempelvis definiera en per databas.</span><span class="sxs-lookup"><span data-stu-id="0dd55-179">Thus, you can define an alert that looks over all databases, say, rather than defining one per database.</span></span> <span data-ttu-id="0dd55-180">Eller skriva en avisering som använder en sammansatt fråga över flera resurstyper.</span><span class="sxs-lookup"><span data-stu-id="0dd55-180">Or write an alert that uses a composite query over multiple resource types.</span></span> <span data-ttu-id="0dd55-181">Frågor begränsas bara av de data som finns tillgängliga i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0dd55-181">Queries are only limited by the data available in the workspace.</span></span>

<span data-ttu-id="0dd55-182">Log Analytics för SQL Database debiteras baserat på datavolymen i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0dd55-182">Log Analytics for SQL Database is charged for based on the data volume in the workspace.</span></span> <span data-ttu-id="0dd55-183">I den här guiden får skapat du en kostnadsfri arbetsyta, som är begränsad till 500MB per dag.</span><span class="sxs-lookup"><span data-stu-id="0dd55-183">In this tutorial, you created a Free workspace, which is limited to 500MB per day.</span></span> <span data-ttu-id="0dd55-184">När den gränsen har uppnåtts, läggs inte längre data till i arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="0dd55-184">Once that limit is reached data is no longer added to the workspace.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0dd55-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0dd55-185">Next steps</span></span>

<span data-ttu-id="0dd55-186">I den här guiden lärde du dig hur man:</span><span class="sxs-lookup"><span data-stu-id="0dd55-186">In this tutorial you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0dd55-187">Installera och konfigurera Log Analytics (OMS)</span><span class="sxs-lookup"><span data-stu-id="0dd55-187">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="0dd55-188">Använda Log Analytics för att övervaka pooler och databaser</span><span class="sxs-lookup"><span data-stu-id="0dd55-188">Use Log Analytics to monitor pools and databases</span></span>

[<span data-ttu-id="0dd55-189">Guide för klientanalys</span><span class="sxs-lookup"><span data-stu-id="0dd55-189">Tenant analytics tutorial</span></span>](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a><span data-ttu-id="0dd55-190">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="0dd55-190">Additional resources</span></span>

* [<span data-ttu-id="0dd55-191">Ytterligare självstudier som bygger på den första Wingtip SaaS-programdistributionen</span><span class="sxs-lookup"><span data-stu-id="0dd55-191">Additional tutorials that build upon the initial Wingtip SaaS application deployment</span></span>](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [<span data-ttu-id="0dd55-192">Azure Log Analytics</span><span class="sxs-lookup"><span data-stu-id="0dd55-192">Azure Log Analytics</span></span>](../log-analytics/log-analytics-azure-sql.md)
* [<span data-ttu-id="0dd55-193">OMS</span><span class="sxs-lookup"><span data-stu-id="0dd55-193">OMS</span></span>](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
