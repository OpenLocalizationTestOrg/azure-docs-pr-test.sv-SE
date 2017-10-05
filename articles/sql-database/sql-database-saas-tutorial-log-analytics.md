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
# <a name="setup-and-use-log-analytics-oms-with-the-wingtip-saas-app"></a>Konfigurera och använda logganalys (OMS) med Wingtip SaaS-appen

I kursen får du konfigurera och använda *logganalys ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* för övervakning av elastiska pooler och databaser. Den bygger på [guiden prestandaövervakning och hantering](sql-database-saas-tutorial-performance-monitoring.md) och visar hur du använder *Log Analytics* för att utöka den övervakning och avisering som finns i Azure-portalen. Log Analytics är särskilt lämpligt för övervakning och avisering i skala eftersom den stöder hundratals pooler och hundratusentals databaser. Den erbjuder också en enskild övervakningslösning som kan integrera övervakning av olika program och Azure-tjänster över flera Azure-prenumerationer.

I den här guiden får du lära du dig hur man:

> [!div class="checklist"]
> * Installera och konfigurera Log Analytics (OMS)
> * Använda Log Analytics för att övervaka pooler och databaser

Följande krav måste uppfyllas för att kunna köra den här självstudiekursen:

* Wingtip SaaS-appen har distribuerats. För att distribuera på mindre än fem minuter finns [distribuera och utforska Wingtip SaaS-program](sql-database-saas-tutorial.md)
* Azure PowerShell ska ha installerats. Mer information finns i [Kom igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

Se [guiden prestandaövervakning och hantering](sql-database-saas-tutorial-performance-monitoring.md) för en diskussion om SaaS-scenarierna och mönstren och hur de påverkar kraven för en övervakningslösning.

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Övervaka och hantera prestanda med Log Analytics (OMS)

För SQL Database, finns övervakning och avisering tillgängligt för databaser och pooler. Den här inbyggda övervakningen och aviseringen är resursspecifik och praktiskt för små antal resurser, men lämpar sig sämre för övervakning av stora installationer eller för att ge en enhetlig vy över olika resurser och prenumerationer.

För scenarion med stora volymer, kan Log Analytics användas. Det här är en separat Azure-tjänst som tillhandahåller analys för utgivna diagnostikloggar och telemetri som samlats in i en arbetsyta för logganalys. Den samlar telemetri från flera olika tjänster som sedan kan användas för att fråga och ställa in aviseringar. Log Analytics tillhandahåller ett inbyggt frågespråk och verktyg för datavisualisering som tillåter operationell dataanalys och visualisering. SQL Analytics-lösningen innehåller flera fördefinierade övervaknings- och aviseringsvyer för elastiska pooler och databasövervakning och avisering och frågor. Den låter dig lägga till dina egna ad-hoc-frågor och spara dem efter behov. OMS innehåller också en designer för anpassade vyer.

Log Analytics arbetsytor och analyslösningar kan öppnas både i Azure-portalen och i OMS. Azure-portalen är den nyare åtkomstpunkten, men kan ligga efter OMS-portalen på vissa områden.

### <a name="start-the-load-generator-to-create-data-to-analyze"></a>Starta belastningsgeneratorn för att skapa data att analysera

1. Öppna **Demo-PerformanceMonitoringAndManagement.ps1** i **PowerShell ISE**. Håll det här skriptet öppet då du kan vilja köra flera scenarion med belastningsgenerering under den här guiden.
1. Om du har mindre än fem klienter, etablerar du en batch med klienter för att skapa en mer intressant övervakningskontext:
   1. Ställ in **$DemoScenario = 1,** **Etablera en batch med klienter**
   1. Tryck **F5** för att köra skriptet.

1. Ställ in **$DemoScenario** = 2, **Generera en belastning av normal intensitet (cirka 40 DTU)**.
1. Tryck **F5** för att köra skriptet.

## <a name="get-the-wingtip-application-scripts"></a>Hämta Wingtip-programskripten

Wingtip biljettskripten och programmets källkod finns tillgängliga från github-repon [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Skriptfilerna finns i [mappen inlärningsmoduler](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Ladda ner mappen **inlärningsmoduler** till din lokala dator med sin mappstruktur intakt.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Installera och konfigurera Log Analytics och sedan Azure SQL Analytics-lösningen

Log Analytics är en separat tjänst som måste konfigureras. Log Analytics samlar in loggdata och telemetri och mått i arbetsyta för logganalys. En arbetsyta är en resurs, precis som andra resurser i Azure och måste skapas. Även om arbetsytan inte behöver skapas i samma resursgrupp som programmen den övervakar, känns det ofta mest logiskt. Detta gör arbetsytan ska raderas enkelt med programmet genom att ta bort resursgruppen för Wingtip SaaS-app.

1. Öppna... \\inlärningsmoduler\\prestandaövervakning och hantering\\Log Analytics\\*Demo-LogAnalytics.ps1* i **PowerShell ISE**.
1. Tryck **F5** för att köra skriptet.

Nu borde du kunna öppna Log Analytics i Azure-portalen (eller OMS-portalen). Det tar några minuter för telemetri att samlas in i Log Analytics-arbetsytan och bli synlig. Ju längre du lämnar systemet med att samla in data, ju mer intressant blir upplevelsen. Ett bra tillfälle att hämta något att dricka. Se bara till att belastningsgeneratorn är igång!


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Använd Log Analytics och SQL Analytics-lösningen för att övervaka pooler och databaser


Öppna Log Analytics och OMS-portalen för att titta på telemetrin som samlas in för databaser och pooler i den här övningen.

1. Bläddra till [Azure-portalen](https://portal.azure.com) och öppna Log Analytics genom att klicka på fler tjänster och därefter söka efter Log Analytics:

   ![öppna log analytics](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. Välj arbetsytan med namnet *wtploganalytics-&lt;användare&gt;*.

1. Välj **översikt** för att öppna Log Analytics-lösningen i Azure-portalen.
   ![översiktslänk](media/sql-database-saas-tutorial-log-analytics/click-overview.png)

    **Viktigt**: Det kan ta några minuter innan lösningen är aktiv. Ha tålamod!

1. Klicka på Azure SQL Analytics-ikonen för att öppna den.

    ![översikt](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analys](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. Vyn i lösningens blad rullar åt sidan, med sin egen rullningslist längst ned (uppdatera bladet vid behov).

1. Utforska de olika vyerna genom att klicka på dem eller på enskilda resurser för att gå in i detalj där du kan använda tidsreglaget i övre vänstra hörnet, eller klicka på en lodrät stapel för att fokusera på ett snävare tidsintervall. Med den här vyn, kan du välja enskilda databaser eller pooler för att fokusera på specifika resurser:

    ![diagram](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. Tillbaka till lösningsbladet, kan du bläddra längst till höger för att se några sparade frågor som du kan klicka på för att öppna och utforska. Du kan experimentera med att ändra dessa och spara intressanta frågor som du skapar, vilka du sedan kan öppna och använda med andra resurser.

1. Tillbaka i Log Analytics-arbetsytebladet, väljer du OMS-portalen för att öppna lösningen där.

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. I OMS-portalen kan du konfigurera aviseringar. Klicka på aviseringsdelen av databasens DTU-vy.

1. I loggsökningsvyn som visas, ser du ett stapeldiagram med de mått som representeras.

    ![loggsökning](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. Om du klickar på avisering i verktygsfältet, kommer att kunna se aviseringskonfigurationen och ändra den.

    ![lägg till aviseringsregel](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

Övervakning och aviseringar i Log Analytics och OMS baseras på frågor över data i arbetsytan, till skillnad från aviseringar på varje resursblad, som är resursspecifika. Därmed kan du definiera en avisering som söker i alla databaser, istället för att exempelvis definiera en per databas. Eller skriva en avisering som använder en sammansatt fråga över flera resurstyper. Frågor begränsas bara av de data som finns tillgängliga i arbetsytan.

Log Analytics för SQL Database debiteras baserat på datavolymen i arbetsytan. I den här guiden får skapat du en kostnadsfri arbetsyta, som är begränsad till 500MB per dag. När den gränsen har uppnåtts, läggs inte längre data till i arbetsytan.


## <a name="next-steps"></a>Nästa steg

I den här guiden lärde du dig hur man:

> [!div class="checklist"]
> * Installera och konfigurera Log Analytics (OMS)
> * Använda Log Analytics för att övervaka pooler och databaser

[Guide för klientanalys](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a>Ytterligare resurser

* [Ytterligare självstudier som bygger på den första Wingtip SaaS-programdistributionen](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
