---
title: "aaaSQL databasen webbprogramutveckling: översikt | Microsoft Docs"
description: "Läs mer om tillgängliga anslutningen bibliotek och bästa praxis för program som ansluter tooSQL databas."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: genemi
ms.assetid: 67c02204-d1bd-4622-acce-92115a7cde03
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: 17f04db600828f90c42c750c9abdb92cfa4ca817
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-database-application-development-overview"></a>SQL Database development Programöversikt
Den här artikeln beskriver hur hello grundläggande överväganden som utvecklare bör vara medveten om när du skriver kod tooconnect tooAzure SQL-databas.

> [!TIP]
> En självstudiekurs visar du hur toocreate en server, skapa en serverbaserad brandvägg Visa serveregenskaper, ansluta med SQL Server Management Studio, fråga hello master-databasen, skapa en exempeldatabas och en tom databas, fråga Databasegenskaper ansluta använder SQL Server Management Studio och fråga hello exempeldatabasen, se [komma igång-Självstudier](sql-database-get-started-portal.md).
>

## <a name="language-and-platform"></a>Språk och plattform
Det finns kodexempel för olika programmeringsspråk och plattformar. Du hittar länkar toohello kodexempel på: 

* Mer information: [Anslutningsbibliotek för SQL Database och SQL Server](sql-database-libraries.md)

## <a name="tools"></a>Verktyg 
Du kan använda verktyg baserade på öppen källkod som [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) och [VS Code](https://code.visualstudio.com/). Azure SQL Database fungerar dessutom med Microsoft-verktyg som [Visual Studio](https://www.visualstudio.com/downloads/) och [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).  Du kan också använda hello Azure-hanteringsportalen, PowerShell och REST API: er hjälpa dig få ytterligare produktivitet.

## <a name="resource-limitations"></a>Resursbegränsningar
Azure SQL Database hanterar hello resurser tillgängliga tooa databasen på två olika sätt: resurser styrning och verkställandet av gränser.

* Mer information: [Resursgränser för Azure SQL Database](sql-database-resource-limits.md)

## <a name="security"></a>Säkerhet
Azure SQL Database innehåller resurser för att begränsa åtkomst, skydda data och övervaka aktiviteter på en SQL Database.

* Mer information: [Säkra din SQL Database](sql-database-security-overview.md)

## <a name="authentication"></a>Autentisering
* Azure SQL Database stöder både användare och inloggningar för SQL Server-autentisering samt användare och inloggningar för [Azure Active Directory-autentisering](sql-database-aad-authentication.md).
* Du behöver toospecify en viss databas i stället för standardpolicy toohello *master* databas.
* Du kan inte använda hello Transact-SQL **Använd myDatabaseName;** instruktionen i SQL-databasen tooswitch tooanother databasen.
* Mer information: [SQL Database-säkerhet: hantera databasåtkomst och inloggningssäkerhet](sql-database-manage-logins.md)

## <a name="resiliency"></a>Återhämtning
När ett tillfälligt fel uppstår vid anslutning tooSQL databasen bör koden försöka hello-anrop.  Rekommenderar vi att logik använda backoff logik så att den inte överväldigande hello SQL-databas med flera klienter som du försöker samtidigt.

* Kodexempel: kodexempel som visar försök logik, se exempel för hello språk på: [anslutningsbibliotek för SQL Database och SQL Server](sql-database-libraries.md)
* Mer information: [Felmeddelanden för SQL Database-klientprogram](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Hantera anslutningar
* I din anslutning logik klienten åsidosätta hello standard timeout toobe 30 sekunder.  hello standardvärdet 15 sekunder är för kort för anslutningar som beror på hello internet.
* Om du använder en [anslutningspool](http://msdn.microsoft.com/library/8xx3tyca.aspx)vara att tooclose hello anslutning hello instant programmet inte aktivt använder det och inte förbereder tooreuse den.

## <a name="network-considerations"></a>Nätverksöverväganden
* Hello-dator som är värd för dina klientprogram, se till att hello brandväggen tillåter utgående TCP-kommunikation på port 1433.  Mer information: [Konfigurera en Azure SQL Database-brandvägg](sql-database-configure-firewall-settings.md)
* Om klientprogrammet ansluter tooSQL databasen när klienten körs på en Azure-dator (VM), måste du öppna vissa portintervall på hello VM. Mer information: [portar utöver 1433 för ADO.NET 4.5 och SQL-databas](sql-database-develop-direct-route-ports-adonet-v12.md)
* Klienten anslutningar tooAzure SQL-databas ibland kringgå hello proxy och interagera direkt med hello-databasen. Andra portar än 1433 blir viktiga. Mer information [Azure SQL Database connectivity arkitektur](sql-database-connectivity-architecture.md) och [portar utöver 1433 för ADO.NET 4.5 och SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Delning av data med elastisk skalbarhet
Elastisk skalbarhet gör hello enklare att skala ut (och i). 

* [Designmönster för SaaS-program med flera klientorganisationer med Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Databeroende routning](sql-database-elastic-scale-data-dependent-routing.md)
* [Kom igång med förhandsversionen av Elastic Scale för Azure SQL Database](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Nästa steg
Utforska alla hello [funktionerna i SQL-databas](sql-database-technical-overview.md)
