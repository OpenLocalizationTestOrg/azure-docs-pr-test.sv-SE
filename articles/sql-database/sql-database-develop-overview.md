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
# <a name="sql-database-application-development-overview"></a><span data-ttu-id="b1803-103">SQL Database development Programöversikt</span><span class="sxs-lookup"><span data-stu-id="b1803-103">SQL Database application development overview</span></span>
<span data-ttu-id="b1803-104">Den här artikeln beskriver hur hello grundläggande överväganden som utvecklare bör vara medveten om när du skriver kod tooconnect tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b1803-104">This article walks through hello basic considerations that a developer should be aware of when writing code tooconnect tooAzure SQL Database.</span></span>

> [!TIP]
> <span data-ttu-id="b1803-105">En självstudiekurs visar du hur toocreate en server, skapa en serverbaserad brandvägg Visa serveregenskaper, ansluta med SQL Server Management Studio, fråga hello master-databasen, skapa en exempeldatabas och en tom databas, fråga Databasegenskaper ansluta använder SQL Server Management Studio och fråga hello exempeldatabasen, se [komma igång-Självstudier](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b1803-105">For a tutorial showing you how toocreate a server, create a server-based firewall, view server properties, connect using SQL Server Management Studio, query hello master database, create a sample database and a blank database, query database properties, connect using SQL Server Management Studio, and query hello sample database, see [Get Started Tutorial](sql-database-get-started-portal.md).</span></span>
>

## <a name="language-and-platform"></a><span data-ttu-id="b1803-106">Språk och plattform</span><span class="sxs-lookup"><span data-stu-id="b1803-106">Language and platform</span></span>
<span data-ttu-id="b1803-107">Det finns kodexempel för olika programmeringsspråk och plattformar.</span><span class="sxs-lookup"><span data-stu-id="b1803-107">There are code samples available for various programming languages and platforms.</span></span> <span data-ttu-id="b1803-108">Du hittar länkar toohello kodexempel på:</span><span class="sxs-lookup"><span data-stu-id="b1803-108">You can find links toohello code samples at:</span></span> 

* <span data-ttu-id="b1803-109">Mer information: [Anslutningsbibliotek för SQL Database och SQL Server](sql-database-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="b1803-109">More Information: [Connection libraries for SQL Database and SQL Server](sql-database-libraries.md)</span></span>

## <a name="tools"></a><span data-ttu-id="b1803-110">Verktyg</span><span class="sxs-lookup"><span data-stu-id="b1803-110">Tools</span></span> 
<span data-ttu-id="b1803-111">Du kan använda verktyg baserade på öppen källkod som [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli) och [VS Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="b1803-111">You can leverage open source tools like [cheetah](https://github.com/wunderlist/cheetah), [sql-cli](https://www.npmjs.com/package/sql-cli), [VS Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="b1803-112">Azure SQL Database fungerar dessutom med Microsoft-verktyg som [Visual Studio](https://www.visualstudio.com/downloads/) och [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span><span class="sxs-lookup"><span data-stu-id="b1803-112">Additionally, Azure SQL Database works with Microsoft tools like [Visual Studio](https://www.visualstudio.com/downloads/) and  [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).</span></span>  <span data-ttu-id="b1803-113">Du kan också använda hello Azure-hanteringsportalen, PowerShell och REST API: er hjälpa dig få ytterligare produktivitet.</span><span class="sxs-lookup"><span data-stu-id="b1803-113">You can also use hello Azure Management Portal, PowerShell, and REST APIs help you gain additional productivity.</span></span>

## <a name="resource-limitations"></a><span data-ttu-id="b1803-114">Resursbegränsningar</span><span class="sxs-lookup"><span data-stu-id="b1803-114">Resource limitations</span></span>
<span data-ttu-id="b1803-115">Azure SQL Database hanterar hello resurser tillgängliga tooa databasen på två olika sätt: resurser styrning och verkställandet av gränser.</span><span class="sxs-lookup"><span data-stu-id="b1803-115">Azure SQL Database manages hello resources available tooa database using two different mechanisms: Resources Governance and Enforcement of Limits.</span></span>

* <span data-ttu-id="b1803-116">Mer information: [Resursgränser för Azure SQL Database](sql-database-resource-limits.md)</span><span class="sxs-lookup"><span data-stu-id="b1803-116">More Information: [Azure SQL Database resource limits](sql-database-resource-limits.md)</span></span>

## <a name="security"></a><span data-ttu-id="b1803-117">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="b1803-117">Security</span></span>
<span data-ttu-id="b1803-118">Azure SQL Database innehåller resurser för att begränsa åtkomst, skydda data och övervaka aktiviteter på en SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b1803-118">Azure SQL Database provides resources for limiting access, protecting data, and monitoring activities on a SQL Database.</span></span>

* <span data-ttu-id="b1803-119">Mer information: [Säkra din SQL Database](sql-database-security-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b1803-119">More Information: [Securing your SQL Database](sql-database-security-overview.md)</span></span>

## <a name="authentication"></a><span data-ttu-id="b1803-120">Autentisering</span><span class="sxs-lookup"><span data-stu-id="b1803-120">Authentication</span></span>
* <span data-ttu-id="b1803-121">Azure SQL Database stöder både användare och inloggningar för SQL Server-autentisering samt användare och inloggningar för [Azure Active Directory-autentisering](sql-database-aad-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="b1803-121">Azure SQL Database supports both SQL Server authentication users and logins, as well as [Azure Active Directory authentication](sql-database-aad-authentication.md) users and logins.</span></span>
* <span data-ttu-id="b1803-122">Du behöver toospecify en viss databas i stället för standardpolicy toohello *master* databas.</span><span class="sxs-lookup"><span data-stu-id="b1803-122">You need toospecify a particular database, instead of defaulting toohello *master* database.</span></span>
* <span data-ttu-id="b1803-123">Du kan inte använda hello Transact-SQL **Använd myDatabaseName;** instruktionen i SQL-databasen tooswitch tooanother databasen.</span><span class="sxs-lookup"><span data-stu-id="b1803-123">You cannot use hello Transact-SQL **USE myDatabaseName;** statement on SQL Database tooswitch tooanother database.</span></span>
* <span data-ttu-id="b1803-124">Mer information: [SQL Database-säkerhet: hantera databasåtkomst och inloggningssäkerhet](sql-database-manage-logins.md)</span><span class="sxs-lookup"><span data-stu-id="b1803-124">More information: [SQL Database security: Manage database access and login security](sql-database-manage-logins.md)</span></span>

## <a name="resiliency"></a><span data-ttu-id="b1803-125">Återhämtning</span><span class="sxs-lookup"><span data-stu-id="b1803-125">Resiliency</span></span>
<span data-ttu-id="b1803-126">När ett tillfälligt fel uppstår vid anslutning tooSQL databasen bör koden försöka hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="b1803-126">When a transient error occurs while connecting tooSQL Database, your code should retry hello call.</span></span>  <span data-ttu-id="b1803-127">Rekommenderar vi att logik använda backoff logik så att den inte överväldigande hello SQL-databas med flera klienter som du försöker samtidigt.</span><span class="sxs-lookup"><span data-stu-id="b1803-127">We recommend that retry logic use backoff logic, so that it does not overwhelm hello SQL Database with multiple clients retrying simultaneously.</span></span>

* <span data-ttu-id="b1803-128">Kodexempel: kodexempel som visar försök logik, se exempel för hello språk på: [anslutningsbibliotek för SQL Database och SQL Server](sql-database-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="b1803-128">Code samples:  For code samples that illustrate retry logic, see samples for hello language of your choice at: [Connection libraries for SQL Database and SQL Server](sql-database-libraries.md)</span></span>
* <span data-ttu-id="b1803-129">Mer information: [Felmeddelanden för SQL Database-klientprogram](sql-database-develop-error-messages.md)</span><span class="sxs-lookup"><span data-stu-id="b1803-129">More information: [Error messages for SQL Database client programs](sql-database-develop-error-messages.md)</span></span>

## <a name="managing-connections"></a><span data-ttu-id="b1803-130">Hantera anslutningar</span><span class="sxs-lookup"><span data-stu-id="b1803-130">Managing connections</span></span>
* <span data-ttu-id="b1803-131">I din anslutning logik klienten åsidosätta hello standard timeout toobe 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="b1803-131">In your client connection logic, override hello default timeout toobe 30 seconds.</span></span>  <span data-ttu-id="b1803-132">hello standardvärdet 15 sekunder är för kort för anslutningar som beror på hello internet.</span><span class="sxs-lookup"><span data-stu-id="b1803-132">hello default of 15 seconds is too short for connections that depend on hello internet.</span></span>
* <span data-ttu-id="b1803-133">Om du använder en [anslutningspool](http://msdn.microsoft.com/library/8xx3tyca.aspx)vara att tooclose hello anslutning hello instant programmet inte aktivt använder det och inte förbereder tooreuse den.</span><span class="sxs-lookup"><span data-stu-id="b1803-133">If you are using a [connection pool](http://msdn.microsoft.com/library/8xx3tyca.aspx), be sure tooclose hello connection hello instant your program is not actively using it, and is not preparing tooreuse it.</span></span>

## <a name="network-considerations"></a><span data-ttu-id="b1803-134">Nätverksöverväganden</span><span class="sxs-lookup"><span data-stu-id="b1803-134">Network considerations</span></span>
* <span data-ttu-id="b1803-135">Hello-dator som är värd för dina klientprogram, se till att hello brandväggen tillåter utgående TCP-kommunikation på port 1433.</span><span class="sxs-lookup"><span data-stu-id="b1803-135">On hello computer that hosts your client program, ensure hello firewall allows outgoing TCP communication on port 1433.</span></span>  <span data-ttu-id="b1803-136">Mer information: [Konfigurera en Azure SQL Database-brandvägg](sql-database-configure-firewall-settings.md)</span><span class="sxs-lookup"><span data-stu-id="b1803-136">More information: [Configure an Azure SQL Database firewall](sql-database-configure-firewall-settings.md)</span></span>
* <span data-ttu-id="b1803-137">Om klientprogrammet ansluter tooSQL databasen när klienten körs på en Azure-dator (VM), måste du öppna vissa portintervall på hello VM.</span><span class="sxs-lookup"><span data-stu-id="b1803-137">If your client program connects tooSQL Database while your client runs on an Azure virtual machine (VM), you must open certain port ranges on hello VM.</span></span> <span data-ttu-id="b1803-138">Mer information: [portar utöver 1433 för ADO.NET 4.5 och SQL-databas](sql-database-develop-direct-route-ports-adonet-v12.md)</span><span class="sxs-lookup"><span data-stu-id="b1803-138">More information: [Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md)</span></span>
* <span data-ttu-id="b1803-139">Klienten anslutningar tooAzure SQL-databas ibland kringgå hello proxy och interagera direkt med hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="b1803-139">Client connections tooAzure SQL Database sometimes bypass hello proxy and interact directly with hello database.</span></span> <span data-ttu-id="b1803-140">Andra portar än 1433 blir viktiga.</span><span class="sxs-lookup"><span data-stu-id="b1803-140">Ports other than 1433 become important.</span></span> <span data-ttu-id="b1803-141">Mer information [Azure SQL Database connectivity arkitektur](sql-database-connectivity-architecture.md) och [portar utöver 1433 för ADO.NET 4.5 och SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="b1803-141">For more information, [Azure SQL Database connectivity architecture](sql-database-connectivity-architecture.md) and [Ports beyond 1433 for ADO.NET 4.5 and SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>

## <a name="data-sharding-with-elastic-scale"></a><span data-ttu-id="b1803-142">Delning av data med elastisk skalbarhet</span><span class="sxs-lookup"><span data-stu-id="b1803-142">Data sharding with elastic scale</span></span>
<span data-ttu-id="b1803-143">Elastisk skalbarhet gör hello enklare att skala ut (och i).</span><span class="sxs-lookup"><span data-stu-id="b1803-143">Elastic Scale simplifies hello process of scaling out (and in).</span></span> 

* [<span data-ttu-id="b1803-144">Designmönster för SaaS-program med flera klientorganisationer med Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b1803-144">Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database</span></span>](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [<span data-ttu-id="b1803-145">Databeroende routning</span><span class="sxs-lookup"><span data-stu-id="b1803-145">Data dependent routing</span></span>](sql-database-elastic-scale-data-dependent-routing.md)
* [<span data-ttu-id="b1803-146">Kom igång med förhandsversionen av Elastic Scale för Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b1803-146">Get Started with Azure SQL Database Elastic Scale Preview</span></span>](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a><span data-ttu-id="b1803-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b1803-147">Next steps</span></span>
<span data-ttu-id="b1803-148">Utforska alla hello [funktionerna i SQL-databas](sql-database-technical-overview.md)</span><span class="sxs-lookup"><span data-stu-id="b1803-148">Explore all hello [capabilities of SQL Database](sql-database-technical-overview.md)</span></span>
