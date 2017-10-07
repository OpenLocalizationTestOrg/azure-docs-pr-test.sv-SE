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
# <a name="setup-and-use-log-analytics-oms-with-hello-wingtip-saas-app"></a>Konfigurera och använda logganalys (OMS) med hello Wingtip SaaS-app

I kursen får du konfigurera och använda *logganalys ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* för övervakning av elastiska pooler och databaser. Den bygger på hello [prestandaövervakning och hantering av kursen](sql-database-saas-tutorial-performance-monitoring.md), och visar hur toouse *logganalys* tooaugment hello övervakning och avisering enligt hello Azure-portalen. Log Analytics är särskilt lämpligt för övervakning och avisering i skala eftersom den stöder hundratals pooler och hundratusentals databaser. Den erbjuder också en enskild övervakningslösning som kan integrera övervakning av olika program och Azure-tjänster över flera Azure-prenumerationer.

I den här guiden får du lära du dig hur man:

> [!div class="checklist"]
> * Installera och konfigurera Log Analytics (OMS)
> * Använd logganalys toomonitor pooler och databaser

den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:

* hello Wingtip SaaS appen har distribuerats. toodeploy på mindre än fem minuter finns [distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)
* Azure PowerShell ska ha installerats. Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

Se hello [prestandaövervakning och hantering av kursen](sql-database-saas-tutorial-performance-monitoring.md) en beskrivning av hello SaaS scenarier och mönster och hur de påverkar hello krav på en övervakningslösning.

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Övervaka och hantera prestanda med Log Analytics (OMS)

För SQL Database, finns övervakning och avisering tillgängligt för databaser och pooler. Den här inbyggda övervakning och avisering är resursspecifika och smidig för litet antal resurser, men är mindre väl lämpade toomonitoring större installationer eller för att tillhandahålla en samlad bild över olika resurser och -prenumerationer.

För scenarion med stora volymer, kan Log Analytics användas. Detta är en separat Azure-tjänst som ger analytics över skickade diagnostikloggar och telemetri som samlats in i en log analytics-arbetsyta, som kan samla in telemetri från många tjänster och att använda tooquery och Ställ in aviseringar. Log Analytics tillhandahåller ett inbyggt frågespråk och verktyg för datavisualisering som tillåter operationell dataanalys och visualisering. hello SQL Analytics lösning innehåller flera fördefinierade elastisk pool och databasen övervakning och avisering vyer och frågor och kan du lägga till egna ad hoc-frågor och spara dem efter behov. OMS innehåller också en designer för anpassade vyer.

Loggen arbetsytorna och analytics Analyslösningar kan öppnas i hello Azure-portalen och i OMS. hello Azure-portalen hello senare åtkomstpunkt men kan vara bakom hello OMS-portalen i vissa områden.

### <a name="start-hello-load-generator-toocreate-data-tooanalyze"></a>Starta hello belastningen generator toocreate data tooanalyze

1. Öppna **Demo-PerformanceMonitoringAndManagement.ps1** i hello **PowerShell ISE**. Behåll det här skriptet öppna som du kanske vill toorun flera hello belastningen generation scenarier under den här kursen.
1. Om du har mindre än fem hyresgäster att etablera en grupp med klienter tooprovide en mer intressant övervakning kontext:
   1. Ställ in **$DemoScenario = 1,** **Etablera en batch med klienter**
   1. Tryck på **F5** toorun hello skript.

1. Ställ in **$DemoScenario** = 2, **Generera en belastning av normal intensitet (cirka 40 DTU)**.
1. Tryck på **F5** toorun hello skript.

## <a name="get-hello-wingtip-application-scripts"></a>Hämta hello Wingtip programskript

hello Wingtip biljetter skript och programmets källkod är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen. Skriptfilerna finns i hello [Learning moduler mappen](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Hämta hello **Learning moduler** mappen tooyour lokala datorn, underhålla dess mappstrukturen.

## <a name="installing-and-configuring-log-analytics-and-hello-azure-sql-analytics-solution"></a>Installera och konfigurera logganalys och hello Azure SQL Analytics lösning

Log Analytics är en separat tjänst som behöver toobe konfigurerats. Log Analytics samlar in loggdata och telemetri och mått i arbetsyta för logganalys. En arbetsyta är en resurs, precis som andra resurser i Azure och måste skapas. Medan hello arbetsytan inte behöver toobe som skapats i hello samma resursgrupp som hello-program som den övervakar, detta gör hello bäst. Detta gör hello arbetsytan toobe enkelt bort hello program genom att ta bort resursgruppen hello i hello fallet hello Wingtip SaaS-appen.

1. Öppna... \\Learning moduler\\prestandaövervakning och hantering av\\logganalys\\*Demo-LogAnalytics.ps1* i hello **PowerShell ISE**.
1. Tryck på **F5** toorun hello skript.

Nu bör du kunna öppna logganalys i hello Azure-portalen (eller hello OMS-portalen). Det tar några minuter för telemetri toobe i hello logganalys-arbetsytan och toobecome som är synliga. hello längre du lämnar hello system samla in data hello mer intressant hello-upplevelse blir. Nu är dags toograb dryck - bara kontrollera hello belastningen generator körs fortfarande!


## <a name="use-log-analytics-and-hello-sql-analytics-solution-toomonitor-pools-and-databases"></a>Använd logganalys och hello SQL Analytics lösning toomonitor pooler och databaser


Öppna Log Analytics och hello OMS-portalen toolook på hello telemetri som samlas in för hello databaser och pooler i den här övningen.

1. Bläddra toohello [Azure-portalen](https://portal.azure.com) och öppna Log Analytics genom att klicka på fler tjänster, och sök sedan efter logganalys:

   ![öppna log analytics](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. Välj hello arbetsyta med namnet *wtploganalytics -&lt;användare&gt;*.

1. Välj **översikt** tooopen hello logganalys lösning i hello Azure-portalen.
   ![översiktslänk](media/sql-database-saas-tutorial-log-analytics/click-overview.png)

    **VIKTIGA**: Det kan ta några minuter innan hello lösningen är aktiv. Ha tålamod!

1. Klicka på hello Azure SQL Analytics panelen tooopen.

    ![översikt](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analys](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. Hej vyn i hello lösning bladet rullar åt sidan, med en egen rullningslist längst hello (uppdatera hello blad om det behövs).

1. Utforska hello olika vyer genom att klicka på dem eller enskilda resurser tooopen en nedåt explorer, där du kan använda hello tid-skjutreglaget i hello vänster överkant eller klicka på ett lodrätt menyraden toofocus i på ett smalare tidsintervallet. Med den här vyn kan välja du enskilda databaser och pooler toofocus på specifika resurser:

    ![diagram](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. Tillbaka på hello lösning bladet om du bläddrar i toohello längst till höger visas några sparade frågor som du kan klicka på tooopen och utforska. Du kan experimentera med att ändra dessa och spara intressanta frågor som du skapar, vilka du sedan kan öppna och använda med andra resurser.

1. Välj OMS-portalen tooopen hello lösning det tillbaka på hello logganalys arbetsytan bladet.

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. Du kan konfigurera aviseringar i hello OMS-portalen. Klicka på hello avisering del av hello databasen DTU vyn.

1. I hello loggen Sök ser vy som visas som du ett stapeldiagram av hello mätvärden som representeras.

    ![loggsökning](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. Om du klickar på avisering i hello verktygsfältet kommer att kunna toosee hello varningskonfigurationen och kan ändra den.

    ![lägg till aviseringsregel](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

hello övervakning och avisering i logganalys och OMS baseras på frågor över hello data hello arbetsytan, till skillnad från hello Varna vid varje resursbladet är resursspecifika. Därmed kan du definiera en avisering som söker i alla databaser, istället för att exempelvis definiera en per databas. Eller skriva en avisering som använder en sammansatt fråga över flera resurstyper. Frågor begränsas bara av hello data är tillgängliga i hello arbetsyta.

Log Analytics för SQL-databas debiteras baserat på hello datavolym hello arbetsytan. I kursen får skapat du en kostnadsfri arbetsyta som är begränsad too500MB per dag. När gränsen har nåtts läggs inte längre data toohello arbetsytan.


## <a name="next-steps"></a>Nästa steg

I den här guiden lärde du dig hur man:

> [!div class="checklist"]
> * Installera och konfigurera Log Analytics (OMS)
> * Använd logganalys toomonitor pooler och databaser

[Guide för klientanalys](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a>Ytterligare resurser

* [Ytterligare självstudier som bygger på hello inledande Wingtip SaaS-programdistribution](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
