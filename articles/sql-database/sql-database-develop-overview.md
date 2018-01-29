---
title: "Översikt över SQL Database-programutveckling | Microsoft Docs"
description: "Läs mer om tillgängliga anslutningsbibliotek och bästa praxis för program som ansluter till SQL Database."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: genemi
ms.assetid: 67c02204-d1bd-4622-acce-92115a7cde03
ms.service: sql-database
ms.custom: develop apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: 5948db9a52dc24d75f3fecc4ed166dd327061b37
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/31/2017
---
# <a name="sql-database-application-development-overview"></a>SQL Database development Programöversikt
Den här artikeln beskriver grundläggande överväganden som utvecklare bör tänka på då de skriver kod för att ansluta till Azure SQL Database.

> [!TIP]
> En genomgång som visar dig hur du skapar en server, skapar en serverbaserad brandvägg, visar egenskaper, ansluter med SQL Server Management Studio, frågar huvuddatabasen, skapar en exempeldatabas och en tom databas, frågar om databasegenskaper, ansluter med SQL Server Management Studio och frågar exempeldatabasen, finns i[	Komma igång - Självstudier](sql-database-get-started-portal.md).
>

## <a name="language-and-platform"></a>Språk och plattform
Det finns kodexempel för olika programmeringsspråk och plattformar. Du hittar länkar till kodexemplen på: 

* Mer information: [anslutningsbibliotek för SQL Database och SQL Server](sql-database-libraries.md).

## <a name="tools"></a>Verktyg 
Du kan använda verktyg baserade på öppen källkod som [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) och [VS Code](https://code.visualstudio.com/). Azure SQL Database fungerar dessutom med Microsoft-verktyg som [Visual Studio](https://www.visualstudio.com/downloads/) och [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).  Du kan också använda Azure-hanteringsportalen, PowerShell och REST API: er för att ytterligare höja produktiviteten.

## <a name="resource-limitations"></a>Resursbegränsningar
Azure SQL Database hanterar de resurser som är tillgängliga för en databas på två olika sätt: resursstyrning och verkställandet av gränser.

* Mer information: [gränserna för Azure SQL Database](sql-database-service-tiers.md).

## <a name="security"></a>Säkerhet
Azure SQL Database innehåller resurser för att begränsa åtkomst, skydda data och övervaka aktiviteter på en SQL Database.

* Mer information: [skydda SQL-databasen](sql-database-security-overview.md).

## <a name="authentication"></a>Autentisering
* Azure SQL Database stöder både användare och inloggningar för SQL Server-autentisering samt användare och inloggningar för [Azure Active Directory-autentisering](sql-database-aad-authentication.md).
* Du måste ange en viss databas i stället för *huvud*databasen som standard.
* Du kan inte använda uttrycket Transact-SQL **USE myDatabaseName;** i SQL Database för att växla till en annan databas.
* Mer information: [SQL Database-säkerhet: hantera åtkomst och logga in databassäkerhet](sql-database-manage-logins.md).

## <a name="resiliency"></a>Återhämtning
När ett tillfälligt fel uppstår vid anslutning till SQL Database, bör din kod göra om anropet.  Vi rekommenderar att logik för omprövning använder begränsningslogik så att den inte överbelastar SQL Database med flera klienter som försöker samtidigt.

* Kodexempel: kodexempel som visar försök logik, se exempel för språket i ditt val på: [anslutningsbibliotek för SQL Database och SQL Server](sql-database-libraries.md).
* Mer information: [felmeddelanden för SQL Database-klientprogram](sql-database-develop-error-messages.md).

## <a name="managing-connections"></a>Hantera anslutningar
* I din klient för anslutningslogik åsidosätter du standardvärdet för timeout till att vara 30 sekunder.  Standardvärdet på 15 sekunder är för kort för anslutningar som beror på internet.
* Om du använder en [anslutningspool](http://msdn.microsoft.com/library/8xx3tyca.aspx), måste du stänga anslutningen så snart programmet inte aktivt använder den och inte förbereder sig för att återanvända den.

## <a name="network-considerations"></a>Nätverksöverväganden
* På den dator som är värd för ditt klientprogram, ska du se till att brandväggen tillåter utgående TCP-kommunikation på port 1433.  Mer information: [konfigurera en Azure SQL Database-brandvägg](sql-database-configure-firewall-settings.md).
* Om klientprogrammet ansluter till SQL-databas när klienten körs på en Azure-dator (VM), måste du öppna vissa portintervall på den virtuella datorn. Mer information: [portar utöver 1433 för ADO.NET 4.5 och SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
* Klientanslutningar till Azure SQL Database ibland kringgå proxyn och interagera direkt med databasen. Andra portar än 1433 blir viktiga. Mer information [Azure SQL Database connectivity arkitektur](sql-database-connectivity-architecture.md) och [portar utöver 1433 för ADO.NET 4.5 och SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Delning av data med elastisk skalbarhet
Elastisk utskalning gör enklare att skala ut (och i). 

* [Designmönster för flera innehavare SaaS-program med Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* [Data beroende routning](sql-database-elastic-scale-data-dependent-routing.md).
* [Komma igång med Azure SQL Database elastisk skalbarhet Preview](sql-database-elastic-scale-get-started.md).

## <a name="next-steps"></a>Nästa steg
Utforska alla [funktionerna i SQL Database](sql-database-technical-overview.md).
