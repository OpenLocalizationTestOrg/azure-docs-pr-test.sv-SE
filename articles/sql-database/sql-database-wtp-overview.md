---
title: "aaaIntro Wingtip SaaS - app för flera innehavare av Azure SQL Database | Microsoft Docs"
description: "Lär dig mer med hjälp av ett exempelprogram för flera innehavare som använder Azure SQL Database, hello Wingtip SaaS-app"
keywords: sql database tutorial
services: sql-database
author: stevestein
manager: jhubbard
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: sstein
ms.openlocfilehash: daeed293116fca22718831b780533be6ef2ad178
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-wingtip-saas-application"></a>Introduktion toohello Wingtip SaaS-program

Hej *Wingtip SaaS* programmet är en exempelapp för flera innehavare som visar hello unika fördelarna med SQL-databas. hello appen använder en databas-per-klient, SaaS programmönster tooservice flera innehavare. hello appen är utformad tooshowcase funktioner i Azure SQL Database som möjliggör SaaS scenarier, inklusive flera SaaS design- och mönster. tooquickly komma igång och körs, hello Wingtip SaaS app distribuerar på mindre än fem minuter!

Programmet källa koden och hantering av skript är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen. toorun hello skript [hello Learning moduler hämtningsmapp](#download-and-unblock-the-wingtip-saas-scripts) tooyour lokal dator.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL-databas Wingtip SaaS-självstudier

När du har distribuerat hello app utforska hello följande kurser som bygger på hello första distributionen. Dessa självstudiekurser utforska vanliga SaaS-mönster som utnyttjar inbyggda funktioner för SQL Database, SQL Data Warehouse och andra Azure-tjänster. Självstudier med PowerShell-skript med förklaringar som förenklar förstå och implementera hello samma SaaS management mönster i dina program.


| Självstudier | Beskrivning |
|:--|:--|
|[Distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)| **BÖRJA HÄR!** Distribuera och utforska hello Wingtip SaaS-program tooyour Azure-prenumeration. |
|[Etablera och katalogen klienter](sql-database-saas-tutorial-provision-and-catalog.md)| Lär dig hur hello program ansluter tootenants med en katalog-databas och hur hello katalog mappar innehavare tootheir data. |
|[Övervaka och hantera prestanda](sql-database-saas-tutorial-performance-monitoring.md)| Lär dig hur toouse övervakning funktioner för SQL-databas och hur tooset aviseringar när tröskelvärden för prestanda har överskridits. |
|[Övervakare med logganalys (OMS)](sql-database-saas-tutorial-log-analytics.md) | Lär dig mer om hur du använder [logganalys](../log-analytics/log-analytics-overview.md) toomonitor stora mängder resurser över flera pooler. |
|[Återställa en enskild klient](sql-database-saas-tutorial-restore-single-tenant.md)| Lär dig hur toorestore en klient databasen tooa tidigare punkt i tid. Steg toorestore tooa parallella databasen, lämnar hello befintlig klient databasen online, ingår också. |
|[Hantera schemat för klient](sql-database-saas-tutorial-schema-management.md)| Lär dig hur tooupdate schemat och uppdatering referensdata, över alla Wingtip SaaS-klienter. |
|[Köra ad hoc-analytics](sql-database-saas-tutorial-adhoc-analytics.md) | Skapa en ad hoc-analytics databas och kör realtid distribuerade frågor över alla klienter.  |
|[Kör klienten analytics](sql-database-saas-tutorial-tenant-analytics.md) | Extrahera klientdata till en analytics databasen eller data warehouse kör offline analytiska frågor. |



## <a name="application-architecture"></a>Programarkitektur

hello Wingtip SaaS app använder hello databas per klient modellen och använder SQL elastiska pooler toomaximize effektivitet. En katalog databas används för etablering och mappa hyresgäster tootheir data. Hej core Wingtip SaaS-program, använder en pool med tre exempel klienter, samt hello katalogdatabasen. Slutför många hello Wingtip SaaS självstudier leda till tillägg toohello första distribution genom att introducera analytiska databaser flera databaser schemahantering, osv.


![Wingtip SaaS-arkitekturen](media/sql-database-wtp-overview/app-architecture.png)


Gå igenom hello självstudier och arbeta med hello app är det viktigt toofocus på hello SaaS mönster relation toohello datanivå. Med andra ord kan du fokusera på hello datanivå och inte över analysera själva hello appen. Förstå hello implementeringen av dessa SaaS-mönster är viktiga tooimplementing dessa mönster i dina program när du funderar på att alla nödvändiga ändringar för dina specifika affärsbehov.

## <a name="download-and-unblock-hello-wingtip-saas-scripts"></a>Hämta och avblockera hello Wingtip SaaS-skript

Körbara innehållet (skript, DLL-filer) kan blockeras av Windows när zip-filer laddas ned från en extern källa och extraheras. När hello skript från en zip-fil, ***gör hello nedan toounblock hello ZIP-filen innan du extraherar***. Detta säkerställer att hello skript tillåts toorun.

1. Bläddra för[github-repo-hello Wingtip SaaS](https://github.com/Microsoft/WingtipSaaS).
1. Klicka på **kloning eller hämta**.
1. Klicka på **hämta ZIP** och spara hello-filen.
1. Högerklicka på hello **WingtipSaaS master.zip** fil och markera **egenskaper**.
1. På hello **allmänna** väljer **avblockera**.
1. Klicka på **OK**.
1. Extrahera hello-filer.

Skript finns i hello *... \\WingtipSaaS master\\Learning moduler* mapp.


## <a name="working-with-hello-wingtip-saas-powershell-scripts"></a>Arbeta med hello Wingtip SaaS PowerShell-skript

tooget hello mest utanför hello exempel måste toodive i hello tillhandahålls skript. Använd brytpunkter och gå igenom hello skript, undersöka hello information om hur hello olika SaaS mönster implementeras. tooeasily stega igenom hello tillhandahålls skript och moduler för hello bäst att förstå, bör du använda hello [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-hello-configuration-file-for-your-deployment"></a>Uppdatera hello konfigurationsfil för distributionen

Redigera hello **UserConfig.psm1** fil med hello resurs grupp- och värdet som anges under distributionen:

1. Öppna hello *PowerShell ISE* och läsa in... \\Learning moduler\\*UserConfig.psm1* 
1. Uppdatera *ResourceGroupName* och *namn* med hello specifika värden för din distribution (på rader 10 och 11 endast).
1. Spara ändringarna för Hej!

Om dessa värden anges här helt enkelt att du inte har tooupdate dessa distribution-specifika värden i alla skript.

### <a name="execute-scripts-by-pressing-f5"></a>Kör skript genom att trycka på F5

Använda flera skript *$PSScriptRoot* toonavigate mappar och *$PSScriptRoot* utvärderas bara när skript körs genom att trycka på **F5**.  Markera och kör en markering (**F8**) kan leda till fel, trycker du på **F5** när du kör skript.

### <a name="step-through-hello-scripts-tooexamine-hello-implementation"></a>Gå igenom hello skript tooexamine hello implementering

hello bästa hur toounderstand hello skript är genom att gå igenom dem toosee vad de gör. Kolla in hello ingår **Demo -** skript som finns ett enkelt toofollow övergripande arbetsflöde. Hej **Demo -** skript visar hello steg krävs tooaccomplish varje aktivitet, så ange brytpunkter och detaljerat djupare hello individuella kräver toosee implementeringsdetaljer hello olika SaaS-mönster.

Tips för att utforska och stega igenom PowerShell-skript:

* Öppna **Demo -** skript i hello PowerShell ISE.
* Köra eller fortsätta med **F5** (med hjälp av **F8** rekommenderas inte eftersom *$PSScriptRoot* utvärderas inte när du kör val av ett skript).
* Placera brytpunkter genom att klicka, eller välja en rad och trycka på **F9**.
* Stega över en funktion eller skriptanrop med **F10**.
* Stega in i en funktion eller skriptanrop med **F11**.
* Steg utanför hello aktuella funktionen eller skriptet anrop med **SKIFT + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Utforska databasschemat och kör SQL-frågor med SSMS

Använd [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) tooconnect och bläddra hello programservrar och databaser.

hello distribution ursprungligen har två SQL Database-servrar tooconnect för-hello *tenants1 -&lt;användare&gt;*  server och hello *katalog -&lt;användare&gt;* server. tooensure en lyckad demo-anslutning, båda servrarna har en [brandväggsregel](sql-database-firewall-configure.md) så att alla IP-adresser via.


1. Öppna *SSMS* och ansluta toohello *tenants1 -&lt;användare&gt;. database.windows.net* server.
1. Klicka på **anslut** > **databasmotor...** :

   ![katalogserver](media/sql-database-wtp-overview/connect.png)

1. Autentiseringsuppgifterna för demot är: inloggningsnamn = *developer*, lösenord = *P@ssword1*

   ![anslutning](media\sql-database-wtp-overview\tenants1-connect.png)

1. Upprepa steg 2 – 3 och ansluta toohello *katalog -&lt;användare&gt;. database.windows.net* server.

När du har anslutit, borde du se bägge servrarna. I listan över databaser kan vara olika beroende på hello klienter du har etablerat:

![objektutforskaren](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Nästa steg

[Distribuera hello Wingtip SaaS-program](sql-database-saas-tutorial.md)
