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
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a>Extrahera data från klienten databaser till en analytics-databas för offlineanalys

I den här kursen använder du en elastiska jobb toorun frågor mot varje klient-databas. hello jobbet extraherar biljett försäljningsdata och läser in den i en analytics-databas (eller ett datalager) för analys. hello analytics-databas är den efterfrågade tooextract insikter från den här dagliga användningsdata för alla klienter.


I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa hello klient analytics-databas
> * Skapa ett schemalagt jobb tooretrieve data och fylla hello analytics-databas

toocomplete den här självstudiekursen, se till att hello följande förutsättningar är uppfyllda:

* hello Wingtip SaaS appen har distribuerats. toodeploy på mindre än fem minuter finns [distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)
* Azure PowerShell ska ha installerats. Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* hello senaste versionen av SQL Server Management Studio (SSMS) är installerad. [Ladda ned och installera SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Klientents användningsanalysmönster

En av hello stora möjligheter med SaaS-program är toouse hello rich-klientdata som lagras i hello molnet. Använd den här data toogain insikter om hello igen och användning av ditt program och dina klienter. Den här informationen hjälper funktionen utveckling, användbarhet förbättringar och andra investeringar i hello appen och plattform. Det är lätt att komma åt denna data när den finns i en enda databas för flera klienter, men inte lika lätt när den är distribuerad i stor skala över potentiellt tusentals databaser. En metod tooaccessing dessa data är toouse elastiska jobb, vilket gör resultatet returneras frågeresultaten från jobbet körning toobe i en utdata databas och tabell.

## <a name="get-hello-wingtip-application-scripts"></a>Hämta hello Wingtip programskript

hello Wingtip SaaS-skript och programmets källkod är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen. [Steg toodownload hello Wingtip SaaS skript](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="deploy-a-database-for-tenant-analytics-results"></a>Distribuera en databas för klientanalysresultat

Den här kursen kräver toohave en databas distribuerade toocapture hello resultat av jobbkörning av skript som innehåller resultatet returneras frågor. Vi skapar en databas som heter tenantanalytics för detta ändamål.

1. Öppna... \\Learning moduler\\operativa Analytics\\klient Analytics\\*Demo-TenantAnalyticsDB.ps1* i hello *PowerShell ISE* och ange Hej följande värde:
   * **$DemoScenario** = **2** *distribuera operativ analysdatabas*
1. Tryck på **F5** toorun hello demoskript (detta anrop hello *distribuera TenantAnalyticsDB.ps1* skript) som skapar hello klient analytics-databas.

## <a name="create-some-data-for-hello-demo"></a>Skapa vissa data för hello demo

1. Öppna... \\Learning moduler\\operativa Analytics\\klient Analytics\\*Demo-TenantAnalyticsDB.ps1* i hello *PowerShell ISE* och ange Hej följande värde:
   * **$DemoScenario** = **1** *köp biljetter för evenemang på alla platser*
1. Tryck på **F5** toorun hello skript och skapa biljett köp historik.


## <a name="create-a-scheduled-job-tooretrieve-tenant-analytics-about-ticket-purchases"></a>Skapa ett schemalagt jobb tooretrieve klient analytics om biljett inköp

Det här skriptet skapar ett jobb tooretrieve biljett Köpinformation från alla klienter. När samman till en tabell får du omfattande insiktsfulla mått om biljett köp mönster över hello innehavare.

1. Öppna SSMS och Anslut toohello katalog -&lt;användaren&gt;. database.windows.net server
1. Öppna... \\inlärningsmoduler\\operativ analys\\klientanalys\\*TicketPurchasesfromAllTenants.sql*
1. Ändra &lt;användare&gt;, Använd hello användarnamnet som används när du distribuerade hello Wingtip SaaS app överst hello hello skriptet **sp\_lägga till\_mål\_grupp\_medlem** och **sp\_lägga till\_jobbsteg**
1. Högerklicka, Välj **anslutning**, och Anslut toohello katalog -&lt;användaren&gt;. database.windows.net server, om inte redan är ansluten
1. Se till att du är ansluten toohello **jobaccount** databasen och tryck på **F5** att köra hello-skript

* **SP\_lägga till\_mål\_grupp** skapar hello målgruppnamn *TenantGroup*, nu måste vi tooadd mål medlemmar.
* **SP\_lägga till\_mål\_grupp\_medlem** lägger till en *server* Medlemstyp som bedömer alla databaser på servern (Obs detta är hello customer1 - mål &lt;Användaren&gt; servern som innehåller hello klient databaser) vid tiden för jobbet körningen ska tas med i hello jobbet.
* **sp\_add\_job** skapar ett nytt jobb som schemaläggs veckovis och heter ”biljettköp från alla klienter”
* **SP\_lägga till\_jobbsteg** skapar hello steg som innehåller T-SQL-kommandot text tooretrieve Köpinformation för alla hello-biljett från alla klienter och kopiera hello returnerar resultat i en tabell med namnet  *AllTicketsPurchasesfromAllTenants*
* hello återstående i hello skript i vyerna hello förekomsten av hello objekt och körning av övervakaren jobb. Granska hello statusvärde från hello **livscykel** kolumnen toomonitor hello status. En gång, lyckades, hello jobbet har slutförts i alla klient-databaser och hello två nya databaser som innehåller hello till tabellen.

Hello skript körs bör resultera i liknande resultat:

![resultat](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-tooretrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Skapa ett jobb tooretrieve sammanfattning antalet biljett inköp från alla klienter

Det här skriptet skapar ett jobb tooretrieve summan av alla biljett inköp från alla klienter.

1. Öppna SSMS och Anslut toohello *katalog -&lt;användare&gt;. database.windows.net* server
1. Öppna hello fil... \\Learning moduler\\etablera och katalogen\\operativa Analytics\\klient Analytics\\*TicketPurchasesfromAllTenants.sql resultat*
1. Ändra &lt;användare&gt;, Använd hello användarnamnet som används när du distribuerade hello Wingtip SaaS-app i hello skriptet i hello **sp\_lägga till\_jobbsteg** lagrad procedur
1. Högerklicka, Välj **anslutning**, och Anslut toohello katalog -&lt;användaren&gt;. database.windows.net server, om inte redan är ansluten
1. Se till att du är ansluten toohello **tenantanalytics** databasen och tryck på **F5** att köra hello-skript

Hello skript körs bör resultera i liknande resultat:

![resultat](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* **sp\_add\_job** skapar en ny schemalagt jobb som heter “ResultsTicketsOrders”

* **SP\_lägga till\_jobbsteg** skapar hello steg som innehåller T-SQL-kommandot text tooretrieve Köpinformation för alla hello-biljett från alla klienter och kopiera hello returnerar resultat i en tabell som kallas CountofTicketOrders

* hello återstående i hello skript i vyerna hello förekomsten av hello objekt och körning av övervakaren jobb. Granska hello statusvärde från hello **livscykel** kolumnen toomonitor hello status. En gång, lyckades, hello jobbet har slutförts i alla klient-databaser och hello två nya databaser som innehåller hello till tabellen.


## <a name="next-steps"></a>Nästa steg

I den här guiden lärde du dig hur man:

> [!div class="checklist"]
> * Distribuera en klientanalysdatabas
> * Skapa ett schemalagt jobb tooretrieve analysdata på klienter

Grattis!

## <a name="additional-resources"></a>Ytterligare resurser

* Ytterligare [självstudier som bygger på hello Wingtip SaaS-program](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastiska jobb](sql-database-elastic-jobs-overview.md)
