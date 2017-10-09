---
title: "aaaGranting åtkomst tooAzure SQL-databas | Microsoft Docs"
description: "Lär dig hur du beviljar åtkomst tooMicrosoft Azure SQL Database."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 8e71b04c-bc38-4153-8f83-f2b14faa31d9
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 02/06/2017
ms.author: rickbyh
ms.openlocfilehash: 4c32fafa7e98b731ff2f9bf4666da7e4a3145286
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-access-control"></a>Azure SQL Database-åtkomstkontroll
tooprovide säkerhet, SQL-databas kontrollerar åtkomsten med brandväggsregler begränsa anslutningen IP-adressen autentiseringsmekanismer som kräver användare tooprove deras identitet och auktorisering mekanismer för att begränsa användare toospecific åtgärder och data. 

> [!IMPORTANT]
> En översikt över hello SQL Database-säkerhetsfunktioner finns [översikt över säkerheten i SQL](sql-database-security-overview.md). En självstudiekurs finns [skydda din Azure SQL Database](sql-database-security-tutorial.md).

## <a name="firewall-and-firewall-rules"></a>Brandvägg och brandväggsregler
Microsoft Azure SQL Database tillhandahåller en relationsdatabastjänst för Azure och andra Internetbaserade program. toohelp skydda dina data, brandväggar förhindra all åtkomst tooyour databasserver förrän du anger vilka datorer som har behörighet. hello brandväggen beviljar åtkomst toodatabases baserat på hello kommer IP-adressen för varje begäran. Mer information finns i [Översikt över Azure SQL Database-brandväggsregler](sql-database-firewall-configure.md)

hello Azure SQL Database-tjänsten är bara tillgängliga via TCP-port 1433. tooaccess en SQL-databas från datorn, se till att klienten datorn brandväggen tillåter att utgående TCP-kommunikation på TCP-port 1433. Om det inte behövs för andra program, blockera inkommande anslutningar på TCP-port 1433. 

Som en del av hello anslutningsprocessen är anslutningar från Azure virtuella datorer omdirigerade tooa annan IP-adress och port, unikt för varje worker-rollen. hello portnumret är hello mellan 11000 too11999. Mer information om TCP-portar finns [portar utöver 1433 för ADO.NET 4.5 och SQL Database2](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Autentisering

SQL Database stöder två typer av autentisering:

* **SQL-autentisering**, som använder ett användarnamn och lösenord. När du skapade hello logisk server för din databas, anges en ”serveradministratören” inloggning med ett användarnamn och lösenord. Med dessa autentiseringsuppgifter, kan du autentisera tooany databasen på den servern som hello databasens ägare eller ”dbo”. 
* **Azure Active Directory-autentisering**, som använder identiteter som hanteras av Azure Active Directory och stöder hanterade och integrerade domäner. Använd Active Directory-autentisering (integrerad säkerhet) [närhelst det går](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode). Om du vill toouse Azure Active Directory-autentisering, måste du skapa en annan serveradministratören kallas hello ”Azure AD admin”, vilket är tillåtet tooadminister Azure AD-användare och grupper. Den här administratören kan också utföra alla åtgärder som en vanlig serveradministratören kan. Se [ansluter tooSQL databasen med hjälp av Azure Active Directory Authentication](sql-database-aad-authentication.md) för en genomgång av hur toocreate en Azure AD admin tooenable Azure Active Directory-autentisering.

hello databasmotorn stänger anslutningar som kan vara inaktiv under mer än 30 minuter. Hej anslutning måste logga in igen innan den kan användas. Kontinuerligt aktiva anslutningar tooSQL databasen kräver omauktorisering (utförs av hello databasmotorn) minst var 10: e timme. hello databasmotorn försöker omauktorisering med hello ursprungligen skickade lösenord och inga indata från användaren krävs. Av prestandaskäl, när en lösenordsåterställning i SQL-databas hello anslutningen är inte autentiseras, även om hello anslutningen återställs på grund av tooconnection poolning. Detta skiljer sig från hello beteendet av lokala SQL Server. Om hello lösenord har ändrats sedan hello anslutning först behörighet, hello anslutning måste avslutas och en ny anslutning görs med hjälp av hello nytt lösenord. En användare med hello `KILL DATABASE CONNECTION` behörighet explicit avsluta en anslutning tooSQL databasen med hjälp av hello [KILL](https://docs.microsoft.com/sql/t-sql/language-elements/kill-transact-sql) kommando.

Användarkonton kan skapas i hello master-databasen och kan tilldelas behörigheter i alla databaser på servern hello. de kan skapas i hello databasen (kallas inneslutna användare) För information om hur man skapar och hanterar inloggningar, se [Hantera inloggningar](sql-database-manage-logins.md). överföring av tooenhance och skalbarhet, använder du innesluten databas användare tooenhance skalbarhet. Mer information om inneslutna användare finns i [Inneslutna databasanvändare – Gör din databas portabel](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), [Skapa användare (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) och [Inneslutna databaser](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases).

Som bästa praxis bör ditt program använda ett särskilt konto tooauthenticate – det här sättet kan du begränsa behörigheterna för hello toohello program och minska hello risker skadlig programvara om din programkod är sårbar tooa SQL injection attack. hello rekommenderad metod är toocreate en [innesluten databas användaren](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable), vilket gör att din app tooauthenticate direkt toohello databas. 

## <a name="authorization"></a>Auktorisering

Auktorisering refererar toowhat en användare kan utföra i en Azure SQL Database och detta styrs av ditt användarkonto databasen [rollmedlemskap](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) och [på objektnivå behörigheter](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine). Som bästa praxis bör du bevilja användare hello lägsta behörighet. administratörskonto för hello servern du ansluter med är medlem i db_owner som har myndigheten toodo något hello-databasen. Spara det här kontot för att distribuera schemauppgraderingar och andra hanteringsåtgärder. Använda hello ”ApplicationUser” konto med mer begränsade behörigheter tooconnect från programmet toohello databasen med hello lägsta privilegier som behövs i programmet. Mer information finns i [Hantera inloggningar](sql-database-manage-logins.md).

Endast administratörer behöver normalt använda toohello `master` databas. Rutinunderhåll åtkomst tooeach användardatabas ska via vanliga oberoende databasanvändare skapas i varje databas. När du använder oberoende databasanvändare, behöver du inte toocreate inloggningar i hello `master` databas. Mer information finns i [Oberoende databasanvändare – göra databasen portabel](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable).

Du bör bekanta dig med följande funktioner som kan använda toolimit eller höja behörighet hello:   
* [Personifiering](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) och [modul-signering](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) kan vara används toosecurely höja behörighet tillfälligt.
* [Säkerhet på radnivå](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kan användas som en begränsning av vilka rader en användare kan komma åt.
* [Datamaskning](sql-database-dynamic-data-masking-get-started.md) kan vara används toolimit exponering av känsliga data.
* [Lagrade procedurer](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) kan vara används toolimit hello-åtgärder som kan göras på hello-databasen.

## <a name="next-steps"></a>Nästa steg

- En översikt över hello SQL Database-säkerhetsfunktioner finns [översikt över säkerheten i SQL](sql-database-security-overview.md).
- toolearn mer information om brandväggsregler, se [regler i brandväggen](sql-database-firewall-configure.md).
- toolearn om användare och inloggningar, se [hantera inloggningar](sql-database-manage-logins.md). 
- En beskrivning av Proaktiv övervakning finns [Database Auditing](sql-database-auditing.md) och [Hotidentifiering för SQL-databasen](sql-database-threat-detection.md).
- En självstudiekurs finns [skydda din Azure SQL Database](sql-database-security-tutorial.md).
