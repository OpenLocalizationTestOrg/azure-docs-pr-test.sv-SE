---
title: "aaaManage Azure SQL Database-schema i en app för flera innehavare | Microsoft Docs"
description: "Hantera scheman för flera klienter i ett program för flera klienter som använder Azure SQL Database"
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
ms.date: 07/28/2017
ms.author: billgib; sstein
ms.openlocfilehash: ea946e556808dabd60dd39cb8173d0512d4bddec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-schema-for-multiple-tenants-in-hello-wingtip-saas-application"></a>Hantera schemat för flera klienter i hello Wingtip SaaS-program

Hej [första Wingtip SaaS-självstudierna](sql-database-saas-tutorial.md) visar hur hello app kan etablera en klient-databas och registreras i hello-katalogen. Precis som alla program kräver hello Wingtip SaaS-app kommer att utvecklas över tid och ibland ändringar toohello databas. Ändringar kan omfatta nya eller ändrade schema, nya eller ändrade referensdata och rutinmässigt Underhåll uppgifter tooensure optimala app databasprestanda. Med SaaS-program behöver ändringarna toobe distribueras på ett samordnat sätt i en potentiellt massiv flotta av klient-databaser. Dessa ändringar toobe i framtiden klient databaser måste de toobe som ingår i hello etableringsprocessen.

Den här självstudiekursen utforskar två scenarier - distribution av referens datauppdateringar för alla klienter och retuning ett index på hello tabell som innehåller hello referensdata. Hej [elastiska jobb](sql-database-elastic-jobs-overview.md) funktionen är används tooexecute dessa åtgärder på alla klienter och hello *gyllene* klient-databas som används som en mall för nya databaser.

I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]

> * Skapa ett konto för jobb
> * Fråga på flera innehavare
> * Uppdaterar data i alla klientdatabaser
> * Skapar ett index i en tabell i alla klientdatabaser


toocomplete den här självstudiekursen, se till att hello följande förutsättningar är uppfyllda:

* hello Wingtip SaaS appen har distribuerats. toodeploy på mindre än fem minuter finns [distribuera och utforska hello Wingtip SaaS-program](sql-database-saas-tutorial.md)
* Azure PowerShell ska ha installerats. Mer information finns i [Komma igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* hello senaste versionen av SQL Server Management Studio (SSMS) är installerad. [Ladda ned och installera SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

*Den här kursen använder funktioner för hello SQL Database-tjänsten som är i en begränsad förhandsgranskning (elastisk databas jobb). Om du inte vill toodo den här självstudiekursen, ange ditt prenumerations-id tooSaaSFeedback@microsoft.com = elastiska jobb Preview med ämnet. När ett meddelande bekräftar att din prenumeration har aktiverats, [ladda ned och installera hello senaste förhandsversionen jobb cmdletar](https://github.com/jaredmoo/azure-powershell/releases). Den här förhandsgranskningen är begränsad, så Kontakta SaaSFeedback@microsoft.com för frågor eller support.*


## <a name="introduction-toosaas-schema-management-patterns"></a>Introduktion tooSaaS mönster för hantering av schemat

hello enstaka klient per databas SaaS mönstret fördelar i många olika sätt från hello isolering av data som är ett resultat, men på hello samtidigt introducerar hello ytterligare komplexitet för underhåll och hantering av flera databaser. [Elastiska jobb](sql-database-elastic-jobs-overview.md) underlättar administrationen och hantering av hello SQL datanivå. Jobb kan du toosecurely och tillförlitligt, köra uppgifter (T-SQL-skript) oberoende av användarinteraktion eller indata, mot en grupp databaser. Den här metoden kan vara används toodeploy schema och gemensamma data ändras över alla klienter i ett program. Elastiska jobb kan också använda toomaintain en *gyllene* kopia av databasen hello används toocreate nya klienter att kontrollera det alltid har hello senaste schema- och data.

![skärmen](media/sql-database-saas-tutorial-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>Elastiska jobb begränsad förhandsvisning

Det finns en ny version av elastiska jobb som nu är en inbyggd funktion i Azure SQL Database (som inte kräver några ytterligare tjänster eller komponenter). Den här nya versionen av elastiska jobb är för närvarande i begränsad förhandsvisning. Den här begränsade förhandsgranskning för närvarande stöder PowerShell toocreate jobbet konton och T-SQL toocreate och hantera jobb.

> [!NOTE]
> *Den här kursen använder funktioner för hello SQL Database-tjänsten som är i en begränsad förhandsgranskning (elastisk databas jobb). Om du inte vill toodo den här självstudiekursen, ange ditt prenumerations-id tooSaaSFeedback@microsoft.com = elastiska jobb Preview med ämnet. När ett meddelande bekräftar att din prenumeration har aktiverats, [ladda ned och installera hello senaste förhandsversionen jobb cmdletar](https://github.com/jaredmoo/azure-powershell/releases). Den här förhandsgranskningen är begränsad, så Kontakta SaaSFeedback@microsoft.com för frågor eller support.*

## <a name="get-hello-wingtip-application-scripts"></a>Hämta hello Wingtip programskript

hello Wingtip SaaS-skript och programmets källkod är tillgängliga i hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-lagringsplatsen. [Steg toodownload hello Wingtip SaaS skript](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-a-job-account-database-and-new-job-account"></a>Skapa en jobbkonto-databas och ett nytt jobbkonto

Den här kursen kräver att du använder PowerShell toocreate hello jobbet databasen och jobbet tjänstkontot. Som MSDB och SQL Agent elastiska jobb använder en Azure SQL-databas toostore jobbdefinitioner, jobbstatus och tidigare. När hello jobbet konto har skapats kan du skapa och övervaka jobb omedelbart.

1. Öppna... \\Learning moduler\\schemat Management\\*Demo-SchemaManagement.ps1* i hello **PowerShell ISE**.
1. Tryck på **F5** toorun hello skript.

Hej *Demo-SchemaManagement.ps1* skript anrop hello *distribuera SchemaManagement.ps1* skript toocreate en *S2* databas med namnet **jobaccount** på hello katalogserver. Sedan skapar den hello jobbet konto, skickar hello jobaccount databasen som en parameter toohello jobbet kontot skapa-anrop.

## <a name="create-a-job-toodeploy-new-reference-data-tooall-tenants"></a>Skapa ett jobb toodeploy ny referens data tooall klienter

Varje klient-databasen innehåller en uppsättning platsen typer som definierar hello slags händelser som finns på en plats. I den här övningen du distribuerar en uppdatering tooall hello klient databaser tooadd två ytterligare plats typer: *motorcykel tävling* och *simning en*. Dessa typer av platsen motsvarar toohello bakgrundsbilden som du ser i hello klient händelser app.

Klicka på hello platsen typen nedrullningsbara menyn och verifiera att endast 10 platsen alternativ är tillgängliga och specifikt som motorcykel tävling och 'Simning en' ingår inte i hello lista.

Nu skapar vi ett jobb tooupdate hello *VenueTypes* tabell i alla hello klient databaser och lägga till hello nya typer av platsen.

toocreate ett nytt jobb, vi använder en uppsättning jobb system lagrade procedurer som skapats i hello jobaccount databasen när hello jobbet kontot skapades.

1. Öppna SSMS och Anslut toohello katalogserver: katalog -\<användaren\>. database.windows.net server
1. Även ansluta toohello klient server: tenants1 -\<användaren\>. database.windows.net
1. Bläddra toohello *contosoconcerthall* databasen på hello *tenants1* server och fråga hello *VenueTypes* tabellen tooconfirm som *motorcykel tävling*  och *simning en* **inte** i hello resultatlistan.
1. Öppna hello fil... \\Learning moduler\\schemat Management\\DeployReferenceData.sql
1. Ändra hello-uttryck: Ange @wtpUser = &lt;användaren&gt; och ersätta hello användaren värdet används när du har distribuerat hello Wingtip app
1. Se till att du är ansluten toohello jobaccount databasen och tryck på **F5** att köra hello-skript

* **SP\_lägga till\_mål\_grupp** skapar hello målgruppnamn DemoServerGroup, nu måste vi tooadd mål medlemmar.
* **SP\_lägga till\_mål\_grupp\_medlem** lägger till en *server* mål Medlemstyp som bedömer alla databaser på servern (Obs detta är hello tenants1-&lt; Användaren&gt; servern som innehåller hello klient databaser) vid tiden för jobbet körningen ska tas med i hello jobbet hello andra är att lägga till en *databasen* måltyp medlem, särskilt hello (”gyllene-databas basetenantdb) som finns i katalogen -&lt;användare&gt; server, och slutligen en annan *databasen* grupp medlem typen tooinclude hello adhocanalytics måldatabasen som används i en senare vägledning.
* **sp\_add\_job** skapar ett jobb som heter referensdatadistribution
* **SP\_lägga till\_jobbsteg** skapar hello steg som innehåller T-SQL-kommandot text tooupdate hello referenstabellen, VenueTypes
* hello återstående i hello skript i vyerna hello förekomsten av hello objekt och körning av övervakaren jobb. Använd dessa frågor tooreview hello statusvärde i hello **livscykel** kolumnen toodetermine när hello jobbet har slutförts på alla klient-databaser och hello två nya databaser som innehåller hello referenstabellen.

1. Bläddra i SSMS, toohello *contosoconcerthall* databasen på hello *tenants1* server och fråga hello *VenueTypes* tabellen tooconfirm som *motorcykel Tävling* och *simning en* **är** nu i hello resultatlistan.


## <a name="create-a-job-toomanage-hello-reference-table-index"></a>Skapa ett jobb toomanage hello referens Tabellindex

Liknande toohello föregående övningen den här övningen skapar ett jobb toorebuild hello index på hello referens tabellens primärnyckel, en typisk management databasåtgärd administratör kan utföra efter en belastningen för stora mängder data i en tabell.

Skapa ett jobb med hjälp av hello samma jobb ”system” lagrade procedurer.

1. Öppna SSMS och Anslut toohello katalog -&lt;användaren&gt;. database.windows.net server
1. Öppna hello fil... \\Learning moduler\\schemat Management\\OnlineReindex.sql
1. Högerklicka på anslutningen och välj ansluta toohello katalog -&lt;användaren&gt;. database.windows.net server, om inte redan är ansluten
1. Se till att du är ansluten toohello jobaccount databasen och tryck på F5 toorun hello skript

* sp\_add\_job skapar ett nytt jobb som heter “Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885”
* SP\_lägga till\_jobbsteg skapar hello steg som innehåller T-SQL-kommandot text tooupdate hello index




## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]

> * Skapa ett jobb konto tooquery över flera innehavare
> * Uppdaterar data i alla klientdatabaser
> * Skapar ett index i en tabell i alla klientdatabaser

[Ad-hoc analys-guide](sql-database-saas-tutorial-adhoc-analytics.md)


## <a name="additional-resources"></a>Ytterligare resurser

* [Ytterligare självstudier som bygger på hello Wingtip SaaS-programdistribution](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Hantera utskalade molndatabaser](sql-database-elastic-jobs-overview.md)
* [Skapa och hantera utskalade molndatabaser](sql-database-elastic-jobs-create-and-manage.md)
