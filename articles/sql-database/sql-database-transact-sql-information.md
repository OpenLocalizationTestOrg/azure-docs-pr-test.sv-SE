---
title: aaaResolving T-SQL skillnader migrering Azure SQL Database | Microsoft Docs
description: "Transact-SQL-uttryck som inte stöds helt i Azure SQL Database"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: load & move data
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/17/2017
ms.author: rickbyh
ms.openlocfilehash: 3852013338bfdc6c7da9d1d879c54781682bc635
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resolving-transact-sql-differences-during-migration-toosql-database"></a>Lösa Transact-SQL-skillnader under migreringen tooSQL databas   
När [migrera din databas](sql-database-cloud-migrate.md) från SQL Server tooAzure SQL Server, kan du upptäcka att databasen kräver vissa omarbetningar innan hello SQL Server kan migreras. Det här avsnittet innehåller riktlinjer tooassist både utför den här omarbetningar och förstå varför underliggande orsaker hello hello omarbetningar krävs. toodetect inkompatibiliteter använda hello [Data migrering Assistant (DMA)](https://www.microsoft.com/download/details.aspx?id=53595).

## <a name="overview"></a>Översikt
De flesta Transact-SQL-funktioner som använder program stöds helt i både Microsoft SQL Server och Azure SQL Database. Till exempel hello SQL kärnkomponenter som datatyper, operatorer, string, aritmetiska, logiska, och markören funktioner fungera på samma sätt i SQL Server och SQL-databas. Det finns dock några skillnader i T-SQL i DDL (data definition language) och DML (data manipulation language)-element som resulterar i T-SQL-uttryck och frågor som bara delvis (som senare i det här avsnittet tar vi upp).

Dessutom innehåller vissa funktioner och syntax som inte stöds i alla eftersom Azure SQL Database är utformad tooisolate funktioner från beroenden på hello master-databasen och hello-operativsystem. Därför är de flesta servernivå aktiviteter olämpligt för SQL-databas. T-SQL-uttryck och alternativ är inte tillgängliga om de konfigurera alternativ för servernivå komponenter för operativsystemet, eller ange filen systemkonfigurationen. När sådana funktioner krävs ett lämpligt alternativ ofta är tillgänglig på annat sätt från SQL-databas eller en annan Azure-funktion eller tjänst. 

Till exempel är hög tillgänglighet inbyggd i Azure, så konfigurerar alltid på inte behövs (även om du tooconfigure aktiv geo-replikering för snabbare återställning i hello-händelse en katastrof). T-SQL-uttryck relaterade så tooavailability grupper stöds inte av SQL-databas och hello hantering av dynamiska vyer relaterade tooAlways på även stöds inte.

En lista över hello-funktioner som stöds och som inte stöds av SQL-databas finns [Azure SQL Database funktionsjämförelse](sql-database-features.md). hello listan på sidan kompletterar avsnittet riktlinjer och funktioner och fokuserar på Transact-SQL-uttryck.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Transact-SQL-syntaxen instruktioner med partiellt skillnader
hello core DDL (data definition language)-uttryck är tillgängliga, men vissa DDL-instruktioner har tillägg relaterade toodisk placering och funktioner som inte stöds. 

- Skapa och ändra databas har över tre 12 alternativ. hello rapporterna innehålla filplacering FILESTREAM och alternativ för service broker som endast gäller tooSQL Server. Detta kan ingen roll om du skapar databaser innan du migrerar, men om du migrerar T-SQL-kod som skapar databaser du jämföra [CREATE DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) med hello SQL Server-syntax vid [skapa DATABAS (SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx) toomake att alla hello-alternativ som du använder stöds. Skapa databas för Azure SQL Database har också tjänstmålet och elastisk skalbarhet alternativen som gäller endast tooSQL databas.
- hello skapa och ALTER TABLE-instruktioner har FileTable alternativ som inte kan användas på SQL-databas eftersom FILESTREAM inte stöds.
- Skapa och ändra inloggnings-satser stöds men SQL Database erbjuder inte alla hello-alternativ. toomake databasen fler bärbara SQL-databasen att du använder innehöll databasanvändare i stället för inloggningar när det är möjligt. Mer information finns i [CREATE/ALTER inloggningen](https://msdn.microsoft.com/library/ms189828.aspx) och [styra och bevilja åtkomst till databasen](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>Transact-SQL-syntaxen stöds inte i SQL Database   
Dessutom relaterade toohello för tooTransact-SQL-satser stöds inte funktioner som beskrivs i [Azure SQL Database funktionsjämförelse](sql-database-features.md), hello följande instruktioner och grupper av rapporter, stöds inte. Därför om din databas toobe migrerat använder något av följande hello funktioner, utveckla nytt T-SQL-tooeliminate dessa T-SQL-funktioner och rapporter.

- Sortering av systemobjekt
- Anslutningsrelaterade: Slutpunktsrapporter, `ORIGINAL_DB_NAME`. SQL-databasen har inte stöd för Windows-autentisering, men stöder hello liknande Azure Active Directory-autentisering. Vissa typer av autentisering kräver hello senaste versionen av SSMS. Mer information finns i [ansluta tooSQL Database eller SQL Data Warehouse med hjälp av Azure Active Directory Authentication](sql-database-aad-authentication.md).
- Frågor mellan databaser med tre eller fyra delnamn. (Skrivskyddade frågor över flera databaser stöds med hjälp av [Elastic Database-fråga](sql-database-elastic-query-overview.md).)
- Länkning av ägarskap mellan databaser, `TRUSTWORTHY`-inställning
- `DATABASEPROPERTY` Använd `DATABASEPROPERTYEX` i stället.
- `EXECUTE AS LOGIN` Använd EXECUTE AS USER i stället.
- Kryptering stöds utom för Extensible Key Management
- Eventing: Händelser, meddelanden om händelser, frågemeddelanden
- Filplacering: Syntax relaterade toodatabase filplacering, storlek och databasfiler som hanteras automatiskt av Microsoft Azure.
- Hög tillgänglighet: Syntax relaterade toohigh tillgänglighet, som hanteras via Microsoft Azure-konto. Detta inkluderar syntax för säkerhetskopiering, återställa, Alltid på, databasspegling, loggöverföring återställningslägen.
- Logga reader: Syntax som förlitar sig på hello Händelseloggläsare, som inte är tillgänglig i SQL Database: Push-replikering, registrering av ändringsdata. SQL Database kan vara en prenumerant för en artikel för push-replikering.
- Funktioner: `fn_get_sql`, `fn_virtualfilestats`,`fn_virtualservernodes`
- Globala temporära tabeller
- : Syntax maskinvarurelaterat toohardware-relaterade serverinställningar: till exempel minne, trådar, processortillhörighet trace flaggor. Använd i stället tjänstenivåer.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`, och fyrdelat namn
- .NET framework: CLR-integrering med SQL Server
- Semantisk sökning
- Autentiseringsuppgifter: Använd [databasbegränsade autentiseringsuppgifter](https://msdn.microsoft.com/library/mt270260.aspx) i stället.
- Servernivåobjekt: serverroller, `IS_SRVROLEMEMBER`, `sys.login_token`. `GRANT`, `REVOKE` och `DENY` av serverbehörigheter inte är tillgängliga även om vissa ersätts med databasbehörigheter. Vissa användbara DMV:er på servernivå har motsvarande DMV:er på databasnivå.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure`-alternativ och `RECONFIGURE`. Vissa alternativ är tillgängliga med [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx).
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL ServerAgent: Syntax som förlitar sig på hello SQL Server Agent eller hello MSDB-databasen: aviseringar, operatorer, centrala hanteringsservrar. Använd skript, till exempel Azure PowerShell i stället.
- SQL Server audit: Använd SQL Database auditing i stället.
- Spårning av SQL Server
- Spårningsflaggor: vissa spårningsflagga objekt har flyttats toocompatibility lägen.
- Felsökning av Transact-SQL
- Utlösare: Serveromfattande eller inloggningsutlösare
- `USE`instruktionen: toochange hello kontexten tooa olika databasen, måste du göra en ny anslutning toohello ny databas.

## <a name="full-transact-sql-reference"></a>Fullständig referens för Transact-SQL
Mer information om Transact-SQL-grammatik, -användning och -exempel finns i [Transact-SQL Reference (Database Engine)](https://msdn.microsoft.com/library/bb510741.aspx) i SQL Server Books Online. 

### <a name="about-hello-applies-to-tags"></a>Om hello ”som gäller för” taggar
hello Transact-SQL referens innehåller avsnitt relaterade tooSQL Server versioner 2008 toohello finns. Under hello avsnittet rubrik finns en ikonfältet, lista hello fyra SQL Server-plattformar och som anger tillämplighet. Till exempel introducerades tillgänglighetsgrupper i SQL Server 2012. Den [CREATE AVAILABILTY GROUP](https://msdn.microsoft.com/library/ff878399.aspx) avsnittet anger hello-instruktion gäller **SQL Server (startar med 2012)**. hello-instruktion gäller inte tooSQL Server 2008, SQL Server 2008 R2, Azure SQL Database, Azure SQL Data Warehouse eller Parallel Data Warehouse.

Hello allmänna ämnet för ett ämne kan användas i en produkt i vissa fall, men det finns mindre skillnader mellan produkter. hello skillnaderna visas på mittpunkter i hello avsnittet efter behov. Hello allmänna ämnet för ett ämne kan användas i en produkt i vissa fall, men det finns mindre skillnader mellan produkter. hello skillnaderna visas på mittpunkter i hello avsnittet efter behov. Till exempel är hello CREATE TRIGGER-avsnittet tillgängliga i SQL-databas. Men hello **alla SERVER** för servernivå utlösare, anger att servernivå utlösare inte kan användas i SQL-databas. Använd databasnivå utlösare i stället.

## <a name="next-steps"></a>Nästa steg

En lista över hello-funktioner som stöds och som inte stöds av SQL-databas finns [Azure SQL Database funktionsjämförelse](sql-database-features.md). hello listan på sidan kompletterar avsnittet riktlinjer och funktioner och fokuserar på Transact-SQL-uttryck.

