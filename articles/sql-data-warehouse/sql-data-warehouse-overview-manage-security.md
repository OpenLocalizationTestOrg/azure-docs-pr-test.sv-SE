---
title: aaaSecure en databas i SQL Data Warehouse | Microsoft Docs
description: "Tips för att skydda en databas i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 8fa2f5ca-4cf5-4418-99a2-4dc745799850
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: 5ef4d760e00be46bfe7808bc95dbe1e4b3a51165
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-database-in-sql-data-warehouse"></a>Skydda en databas i SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Översikt över säkerheten](sql-data-warehouse-overview-manage-security.md)
> * [Autentisering](sql-data-warehouse-authentication.md)
> * [Kryptering (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Kryptering (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Den här artikeln beskriver hur hello grunderna för att skydda din Azure SQL Data Warehouse-databas. I synnerhet i den här artikeln hjälper dig att komma igång med resurser för att begränsa åtkomst, skyddar data och övervakar aktiviteter på en databas.

## <a name="connection-security"></a>Anslutningssäkerhet
Anslutningssäkerhet refererar toohow du begränsa och skydda anslutningar tooyour databas med hjälp av brandväggsregler och krypterad anslutning.

Brandväggsregler som används av både hello server och hello databasen tooreject anslutningsförsök från IP-adresser som inte har uttryckligen godkända. tooallow anslutningar från ditt program eller klienten datorns offentliga IP-adress, måste du först skapa en brandväggsregel på servernivå med hjälp av hello Azure-portalen, REST-API och PowerShell. Som bästa praxis bör du begränsa hello IP-adressintervall tillåts via brandväggen server så mycket som möjligt.  tooaccess Azure SQL Data Warehouse från din lokala dator, kontrollera hello brandväggen på ditt nätverk och en lokal dator tillåter utgående kommunikation på TCP-port 1433.  Mer information finns i [Azure SQL Database-brandvägg][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule], och [sp_set_database_ firewall_rule][sp_set_database_firewall_rule].

Anslutningar tooyour SQL Data Warehouse krypterad som standard.  Ändra anslutning inställningar toodisable kryptering ignoreras.

## <a name="authentication"></a>Autentisering
Autentisering refererar toohow du styrka din identitet när du ansluter toohello databas. SQL Data Warehouse stöder för närvarande SQL Server-autentisering med ett användarnamn och lösenord som en Azure Active Directory. 

När du skapade hello logisk server för din databas, anges en ”serveradministratören” inloggning med ett användarnamn och lösenord. Du kan autentisera tooany databasen på den servern som hello databasens ägare eller ”dbo” via SQL Server-autentisering med dessa autentiseringsuppgifter.

Användare i organisationen bör dock som bästa praxis använda ett annat konto tooauthenticate. Det här sättet kan du begränsa behörigheterna för hello toohello program och minska hello risker skadlig programvara om din programkod är sårbar tooa SQL injection attack. 

toocreate SQL Server-autentiserad användare ansluta toohello **master** databasen på servern med din inloggning för serveradministratör och skapa en ny server-inloggning.  Dessutom är det en bra idé toocreate en användare i hello huvuddatabasen för Azure SQL Data Warehouse-användare. Skapa en användare i master kan en användare toologin med hjälp av verktyg som SSMS utan att ange ett databasnamn.  Det gör dem också toouse hello object explorer tooview alla databaser på en SQLServer.

```sql
-- Connect toomaster database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Anslut tooyour **SQL Data Warehouse-databas** med din inloggning för serveradministratör och skapa en databasanvändare baserat på hello server-inloggning som du nyss skapade.

```sql
-- Connect tooSQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

Om en användare kommer att göra ytterligare åtgärder som till exempel skapa inloggningar eller skapa nya databaser, måste de också toobe tilldelade toohello `Loginmanager` och `dbmanager` roller i hello master-databasen. Mer information om dessa ytterligare roller och autentiserande tooa SQL-databas finns [hantera databaser och inloggningar i Azure SQL Database][Managing databases and logins in Azure SQL Database].  Mer information om Azure AD för SQL Data Warehouse finns [ansluter tooSQL Data Warehouse med hjälp av Azure Active Directory Authentication][Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication].

## <a name="authorization"></a>Auktorisering
Auktorisering refererar toowhat kan du göra i en Azure SQL Data Warehouse-databas och detta styrs av ditt användarkonto rollmedlemskap och behörigheter. Som bästa praxis bör du bevilja användare hello lägsta behörighet. Azure SQL Data Warehouse gör den här enkelt toomanage med roller i T-SQL:

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser tooread data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser toowrite data
```

administratörskonto för hello servern du ansluter med är medlem i db_owner som har myndigheten toodo något hello-databasen. Spara det här kontot för att distribuera schemauppgraderingar och andra hanteringsåtgärder. Använda hello ”ApplicationUser” konto med mer begränsade behörigheter tooconnect från programmet toohello databasen med hello lägsta privilegier som behövs i programmet.

Det finns olika sätt toofurther gränsen vad en användare kan göra med Azure SQL Database:

* Detaljerade [behörigheter] [ Permissions] kan du kontrollen vilka åtgärder som du kan på enskilda kolumner, tabeller, vyer, procedurer och andra objekt i hello-databasen. Använd detaljerade behörigheter toohave hello mest kontroll och ge hello minsta möjliga behörigheter krävs. hello detaljerad behörighet system är ganska komplex och kräver vissa undersökning toouse effektivt.
* [Databasrollerna] [ Database roles] utöver db_datareader och db_datawriter kan använda toocreate mer kraftfulla program användarkonton eller mindre kraftfulla hanteringskonton. hello inbyggda fasta databasroller ger ett enkelt sätt toogrant behörigheter, men kan resultera i att bevilja fler behörigheter än nödvändigt.
* [Lagrade procedurer] [ Stored procedures] kan vara används toolimit hello-åtgärder som kan göras på hello-databasen.

Hantera databaser och logiska servrar från hello klassiska Azure-portalen eller med hjälp av hello Azure Resource Manager API styrs av din portal användarkonto rolltilldelningar. Mer information om det här avsnittet finns [rollbaserad åtkomstkontroll i Azure Portal][Role-based access control in Azure Portal].

## <a name="encryption"></a>Kryptering
Azure SQL Data Warehouse Transparent Data kryptering (TDE) skyddar mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av data i vila.  När du krypterar din databas, krypteras tillhörande säkerhetskopior och transaktionsloggfiler utan några ändringar tooyour program. TDE krypterar hello lagring av en hel databas med en symmetrisk nyckel kallas hello krypteringsnyckeln för databasen. I SQL-databas skyddas hello databaskrypteringsnyckeln av inbyggda servercertifikat. hello inbyggda servercertifikatet är unikt för varje SQL Database-server. Microsoft roteras automatiskt dessa certifikat minst var 90: e dag. hello krypteringsalgoritm som används av SQL Data Warehouse är AES-256. En allmän beskrivning av TDE finns [Transparent datakryptering][Transparent Data Encryption].

Du kan kryptera din databas med hjälp av hello [Azure Portal] [ Encryption with Portal] eller [T-SQL][Encryption with TSQL].

## <a name="next-steps"></a>Nästa steg
Information och exempel på hur du ansluter tooyour SQL Data Warehouse med olika protokoll finns [ansluta tooSQL datalagret][Connect tooSQL Data Warehouse].

<!--Image references-->

<!--Article references-->
[Connect tooSQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[Azure SQL Database firewall]: https://msdn.microsoft.com/library/ee621782.aspx
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx
[Database roles]: https://msdn.microsoft.com/library/ms189121.aspx
[Managing databases and logins in Azure SQL Database]: https://msdn.microsoft.com/library/ee336235.aspx
[Permissions]: https://msdn.microsoft.com/library/ms191291.aspx
[Stored procedures]: https://msdn.microsoft.com/library/ms190782.aspx
[Transparent Data Encryption]: https://msdn.microsoft.com/library/bb934049.aspx
[Azure portal]: https://portal.azure.com/

<!--Other Web references-->
[Role-based access control in Azure Portal]: https://azure.microsoft.com/documentation/articles/role-based-access-control-configure
