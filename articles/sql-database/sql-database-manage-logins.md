---
title: "aaaAzure SQL-inloggningar och användare | Microsoft Docs"
description: "Lär dig mer om hantering av säkerhet för SQL Database, specifikt hur toomanage databas säkerhet för åtkomst och logga in via hello servernivå objektkonto."
keywords: "sql database-säkerhet, hantering av databassäkerhet, inloggningssäkerhet, databassäkerhet, databasåtkomst"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 0a65a93f-d5dc-424b-a774-7ed62d996f8c
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/23/2017
ms.author: rickbyh
ms.openlocfilehash: dff889b9fed09146a10008c0d11ca9e71d91df5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="controlling-and-granting-database-access"></a>Kontrollera och att bevilja åtkomst till databasen

När brandväggsregler har konfigurerats, ansluta personer tooa SQL-databas som en hello administratörskonton, som hello databasens ägare eller som en databasanvändare i hello-databasen.  

>  [!NOTE]  
>  Det här avsnittet gäller tooAzure SQLServer och tooboth SQL Database och SQL Data Warehouse-databaser som skapas på hello Azure SQL-servern. För enkelhetens skull används SQL-databasen när det gäller tooboth SQL Database och SQL Data Warehouse. 
>

> [!TIP]
> En självstudiekurs finns [skydda din Azure SQL Database](sql-database-security-tutorial.md).
>


## <a name="unrestricted-administrative-accounts"></a>Obegränsade administrativa konton
Det finns två administrativa konton (**Serveradministratör** och **Active Directory-administratör**) som fungerar som administratörer. tooidentify kontona administratör för SQLServer, öppna hello Azure-portalen och gå toohello egenskaper för SQLServer.

![SQL-serveradministratörer](./media/sql-database-manage-logins/sql-admins.png)

- **Serveradministratör**   
När du skapar en Azure SQL-server måste du ange en **Inloggning för serveradministratör**. SQLServer skapar kontot som en inloggning i hello master-databasen. Det här kontot ansluter med hjälp av SQL Server-autentisering (användarnamn och lösenord). Endast ett av dessa konton kan finnas.   
- **Azure Active Directory-administratör**   
Ett Azure Active Directory-konto, antingen ett enskilt eller säkerhetsgruppkonto, kan också konfigureras som en administratör. Det är valfritt tooconfigure en Azure AD-administratör, men en Azure AD-administratör måste konfigureras om du vill toouse Azure AD-konton tooconnect tooSQL databas. Mer information om hur du konfigurerar åtkomst till Azure Active Directory finns [ansluta tooSQL Database eller SQL Data Warehouse med hjälp av Azure Active Directory Authentication](sql-database-aad-authentication.md) och [SSMS stöd för Azure AD MFA med SQL Database och SQL Data Warehouse](sql-database-ssms-mfa-authentication.md).
 

Hej **serveradministratören** och **Azure AD admin** konton har hello följande egenskaper:
- Detta är endast hello-konton som automatiskt kan ansluta tooany SQL-databas på hello-servern. (tooconnect tooa användardatabas, andra konton måste antingen vara hello ägare hello databasen eller ha ett användarkonto i hello användardatabas.)
- Dessa konton ange användardatabaser som hello `dbo` användaren och de har alla hello behörigheter i hello användardatabaser. (hello ägaren av en användardatabas anges också hello databasen som hello `dbo` användaren.) 
- Dessa konton inte anger hello `master` databas som hello `dbo` användar- och de har begränsad behörighet i master. 
- Dessa konton inte är medlemmar i hello standard SQL Server `sysadmin` fasta serverrollen som inte är tillgänglig i SQL-databas.  
- Dessa konton kan skapa, ändra och ta bort databaser, inloggningar, användare i huvuddatabasen och brandväggsregler på servernivå.
- Dessa konton kan lägga till och ta bort medlemmar toohello `dbmanager` och `loginmanager` roller.
- Dessa konton kan visa hello `sys.sql_logins` systemtabellen.

### <a name="configuring-hello-firewall"></a>Konfigurera hello-brandväggen
När hello servernivå brandväggen är konfigurerad för en enskild IP-adress eller ett område, hello **SQL-administratören** och hello **Azure Active Directory-administratör** kan ansluta toohello master-databasen och alla hello användardatabaser. hello inledande servernivå brandväggen kan konfigureras via hello [Azure-portalen](sql-database-get-started-portal.md)med hjälp av [PowerShell](sql-database-get-started-powershell.md) eller med hjälp av hello [REST API](https://msdn.microsoft.com/library/azure/dn505712.aspx). När en anslutning upprättats, kan även ytterligare brandväggsregler på servernivå konfigureras med hjälp av [Transact-SQL](sql-database-configure-firewall-settings.md).

### <a name="administrator-access-path"></a>Åtkomstväg för administratör
När hello servernivå brandväggen är korrekt konfigurerad, hello **SQL-administratören** och hello **Azure Active Directory-administratör** kan ansluta med klientverktyg, till exempel SQL Server Management Studio eller SQL Server Data Tools. Endast hello senaste verktyg ger alla hello funktioner och möjligheter. hello följande diagram visar en typisk konfiguration för hello två administratörskonton.

![Åtkomstväg för administratör](./media/sql-database-manage-logins/1sql-db-administrator-access.png)

När du använder en öppen port i brandväggen för hello servernivå kan administratörer ansluta tooany SQL-databas.

### <a name="connecting-tooa-database-by-using-sql-server-management-studio"></a>Ansluter tooa databasen med hjälp av SQL Server Management Studio
En genomgång av hur du skapar en server, en databas, brandväggsregler på servernivå och använder SQL Server Management Studio tooquery en databas finns [Kom igång med Azure SQL Database-servrar, databaser och brandväggsregler med hjälp av hello Azure-portalen SQL Server Management Studio och](sql-database-get-started-portal.md).

> [!IMPORTANT]
> Det rekommenderas att du alltid använder hello senaste versionen av Management Studio tooremain synkroniseras med uppdateringar tooMicrosoft Azure och SQL-databas. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="additional-server-level-administrative-roles"></a>Ytterligare administrativa roller på servernivå
Dessutom toohello servernivå administrativa roller som beskrivits tidigare SQL-databasen innehåller två begränsade administrativa roller i hello huvuddatabasen toowhich användarkonton kan läggas till som bevilja behörighet tooeither skapa databaser eller hantera inloggningar.

### <a name="database-creators"></a>Databasskapare
En av dessa administrativa roller är hello **dbmanager** roll. Medlemmar i den här rollen kan skapa nya databaser. toouse den här rollen kan du skapa en användare i hello `master` databasen och Lägg sedan till hello användaren toohello **dbmanager** databasrollen. toocreate en databas hello användaren måste vara en användare baserat på en SQL Server-inloggning i hello master-databasen eller innehöll databasanvändare baserat på en Azure Active Directory-användare.

1. Med ett administratörskonto, ansluta toohello master-databasen.
2. Valfritt steg: skapa en SQL Server-autentisering-inloggning med hello [skapa inloggningen](https://msdn.microsoft.com/library/ms189751.aspx) instruktionen. Exempel på instruktion:
   
   ```
   CREATE LOGIN Mary WITH PASSWORD = '<strong_password>';
   ```
   
   > [!NOTE]
   > Använd ett starkt lösenord när du skapar en inloggning eller en oberoende databasanvändare. Mer information finns i [Starka lösenord](https://msdn.microsoft.com/library/ms161962.aspx).
    
   tooimprove prestanda inloggningar (servernivå säkerhetsobjekt) cachelagras tillfälligt på hello databasnivå. cache för toorefresh hello autentisering, se [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).

3. Skapa en användare med hjälp av hello i hello master-databasen, [skapa användare](https://msdn.microsoft.com/library/ms173463.aspx) instruktionen. hello-användare kan vara ett Azure Active Directory-autentisering finns databasanvändare (om du har konfigurerat din miljö för Azure AD-autentisering), eller en SQL Server-autentisering finns databasanvändare eller SQL Server-autentisering användare baserat på en SQL Inloggning på Server-autentisering (skapat i föregående steg i hello). Exempel på instruktioner:
   
   ```
   CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
   CREATE USER Tran WITH PASSWORD = '<strong_password>';
   CREATE USER Mary FROM LOGIN Mary; 
   ```

4. Lägg till hello ny användare toohello **dbmanager** databasrollen med hjälp av hello [ALTER ROLE](https://msdn.microsoft.com/library/ms189775.aspx) instruktionen. Exempel på instruktioner:
   
   ```
   ALTER ROLE dbmanager ADD MEMBER Mary; 
   ALTER ROLE dbmanager ADD MEMBER [mike@contoso.com];
   ```
   
   > [!NOTE]
   > Hej dbmanager är en databasroll i master-databasen så att du kan bara lägga till en användare toohello dbmanager databasroll. Du kan inte lägga till en roll för inloggning på servernivå toodatabase-nivå.
    
5. Om det behövs konfigurerar du en brandvägg regeln tooallow hello nya användare tooconnect. (hello ny användare kan omfattas av en befintlig brandväggsregel.)

Nu hello användare kan ansluta toohello master-databasen och kan skapa nya databaser. hello konto skapar hello databas blir hello ägare av hello-databasen.

### <a name="login-managers"></a>Inloggningshanterare
hello är andra administrativa hello inloggningen manager roll. Medlemmar i den här rollen kan skapa nya inloggningar i hello master-databasen. Om du vill kan du slutföra hello samma steg (skapa en inloggning och användare och lägga till en användare toohello **loginmanager** rollen) tooenable en användare toocreate nya inloggningar i hello master. Inloggningar är vanligtvis inte nödvändiga som Microsoft rekommenderar att använda oberoende databasanvändare som autentiserar på hello databasnivå i stället för användare baserat på inloggningar. Mer information finns i [Oberoende databasanvändare – göra databasen portabel](https://msdn.microsoft.com/library/ff929188.aspx).

## <a name="non-administrator-users"></a>Användare som är icke-administratörer
I allmänhet icke-administratörskonton inte behöver kommer åt toohello master-databasen. Skapa oberoende databasanvändare på hello databasnivå med hello [skapa användare (Transact-SQL)](https://msdn.microsoft.com/library/ms173463.aspx) instruktionen. hello-användare kan vara ett Azure Active Directory-autentisering finns databasanvändare (om du har konfigurerat din miljö för Azure AD-autentisering), eller en SQL Server-autentisering finns databasanvändare eller SQL Server-autentisering användare baserat på en SQL Inloggning på Server-autentisering (skapat i föregående steg i hello). Mer information finns i [Oberoende databasanvändare – göra databasen portabel](https://msdn.microsoft.com/library/ff929188.aspx). 

toocreate användare ansluta toohello databasen och köra rapporter liknande toohello följande exempel:

```
CREATE USER Mary FROM LOGIN Mary; 
CREATE USER [mike@contoso.com] FROM EXTERNAL PROVIDER;
```

Endast en av hello administratörer eller hello ägare av hello databasen kan först skapa användare. tooauthorize ytterligare användare toocreate nya användare, ge den valda användaren hello `ALTER ANY USER` behörighet genom att använda ett uttryck som:

```
GRANT ALTER ANY USER tooMary;
```

toogive ytterligare användare fullständig kontroll över hello databas, gör dem medlem hello **db_owner** fasta databasrollen med hello `ALTER ROLE` instruktionen.

> [!NOTE]
> hello vanligaste orsak toocreate databasanvändare som baseras på inloggningar, är när du har SQL Server-autentisering-användare som behöver komma åt toomultiple databaser. Användare baserat på inloggningar är bundet toohello inloggning och endast ett lösenord som sparas för att logga in. Oberoende databasanvändare i individuella databaser är var och en individuella enheter och var och en underhåller ett eget lösenord. Detta kan förvirra oberoende databasanvändare om de inte upprätthåller sina lösenord som identiska.

### <a name="configuring-hello-database-level-firewall"></a>Konfigurera hello databasnivå brandväggen
Som bästa praxis ha icke-administratörer endast åtkomst via hello brandväggen toohello databaser som de använder. I stället för att auktorisera sina IP-adresser via hello servernivå brandväggen och ge dem åtkomst tooall databaser, använda hello [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) instruktionen tooconfigure hello databasnivå brandväggen. hello databasnivå brandväggen kan inte konfigureras med hjälp av hello-portalen.

### <a name="non-administrator-access-path"></a>Åtkomstväg för icke-administratör
När hello databasnivå brandväggen är korrekt konfigurerad, kan hello databasanvändare ansluta med klientverktyg som SQL Server Management Studio eller SQL Server Data Tools. Endast hello senaste verktyg ger alla hello funktioner och möjligheter. hello följande diagram visar en typisk icke-administratör komma åt sökvägen.

![Åtkomstväg för icke-administratör](./media/sql-database-manage-logins/2sql-db-nonadmin-access.png)

## <a name="groups-and-roles"></a>Grupper och roller
Effektiv hantering använder behörigheter toogroups och roller i stället för enskilda användare. 

- När du använder Azure Active Directory-autentisering, lägger du Azure Active Directory-användare i en Azure Active Directory-grupp. Skapa en innesluten databasanvändare för hello grupp. Placera en eller flera användare i en [databasrollen](https://msdn.microsoft.com/library/ms189121) och tilldela sedan [behörigheter](https://msdn.microsoft.com/library/ms191291.aspx) toohello databasrollen.

- När du använder SQL Server-autentisering kan du skapa oberoende databasanvändare i hello-databasen. Placera en eller flera användare i en [databasrollen](https://msdn.microsoft.com/library/ms189121) och tilldela sedan [behörigheter](https://msdn.microsoft.com/library/ms191291.aspx) toohello databasrollen.

hello databasroller kan vara hello inbyggda roller som **db_owner**, **db_ddladmin**, **db_datawriter**, **db_datareader**, **db_denydatawriter**, och **db_denydatareader**. **db_owner** är vanliga toogrant fullständig behörighet tooonly några användare. hello andra fasta databasroller är användbara för att hämta en enkel databas under utveckling snabbt, men rekommenderas inte för de flesta produktionsdatabaserna. Till exempel hello **db_datareader** fasta databasrollen ger läsbehörighet tooevery tabellen i hello-databasen, som vanligtvis är större än är absolut nödvändigt. Det är mycket bättre toouse hello [skapa roll](https://msdn.microsoft.com/library/ms187936.aspx) instruktionen toocreate egna användardefinierade databasrollerna och noggrant tilldela varje roll hello lägsta behörigheter som krävs för hello affärsbehov. När en användare är medlem i flera roller kan aggregera de hello behörigheter för alla.

## <a name="permissions"></a>Behörigheter
Det finns över 100 behörigheter som individuellt kan beviljas eller nekas i SQL Database. Många av de här behörigheterna är kapslade. Till exempel hello `UPDATE` behörighet i ett schema som innehåller hello `UPDATE` behörighet för varje tabell i schemat. I de flesta behörighet system åsidosätter hello denial för en behörighet bidrag. Det kan ta noggrann undersökning toodesign en behörighet system tooproperly skydda databasen på grund av hello kapslade natur och hello antalet behörigheter. Börja med hello listan med behörigheter på [behörigheter (Database Engine)](https://msdn.microsoft.com/library/ms191291.aspx) och granska hello [affisch storlek bild](http://go.microsoft.com/fwlink/?LinkId=229142) hello behörigheter.


### <a name="considerations-and-restrictions"></a>Överväganden och begränsningar
När du hanterar inloggningar och användare i SQL-databas, Tänk hello följande:

* Du måste vara anslutna toohello **master** databas när du kör hello `CREATE/ALTER/DROP DATABASE` instruktioner.   
* Hej databasen användaren motsvarande toohello **serveradministratören** inloggning kan inte ändras eller tas bort. 
* Amerikansk engelska är hello standardspråk för hello **serveradministratören** inloggningen.
* Endast hello administratörer (**serveradministratören** inloggning eller Azure AD-administratör) och hello medlemmar i hello **dbmanager** databasrollen i hello **master** finns i databasen behörighet tooexecute hello `CREATE DATABASE` och `DROP DATABASE` instruktioner.
* Du måste vara anslutna toohello master-databasen när du kör hello `CREATE/ALTER/DROP LOGIN` instruktioner. Att använda inloggningar rekommenderas inte. Använd i stället oberoende databasanvändare.
* tooconnect tooa användardatabas, måste du ange hello namnet på databasen som hello i hello anslutningssträngen.
* Endast hello servernivå UPN-inloggning och hello medlemmar i hello **loginmanager** databasrollen i hello **master** databasen ha behörigheten tooexecute hello `CREATE LOGIN`, `ALTER LOGIN`, och `DROP LOGIN` instruktioner.
* När du kör hello `CREATE/ALTER/DROP LOGIN` och `CREATE/ALTER/DROP DATABASE` instruktioner i ett ADO.NET-program, parameteriserade kommandona tillåts inte. Mer information finns i [Kommandon och parametrar](https://msdn.microsoft.com/library/ms254953.aspx).
* När du kör hello `CREATE/ALTER/DROP DATABASE` och `CREATE/ALTER/DROP LOGIN` rapporter, var och en av de här uttrycken måste vara hello endast instruktionen i en Transact-SQL-batch. Annars uppstår ett fel. Till exempel hello följande Transact-SQL kontrollerar om hello databasen finns. Om det finns en `DROP DATABASE` instruktionen kallas tooremove hello-databasen. Eftersom hello `DROP DATABASE` -instruktionen är inte hello enda instruktionen i hello batch, köra hello följande Transact-SQL-uttryck som resulterar i ett fel.

  ```
  IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
  DROP DATABASE [database_name];
  GO
  ```

* När du kör hello `CREATE USER` instruktion med hello `FOR/FROM LOGIN` alternativ, den måste vara hello endast instruktionen i en Transact-SQL-batch.
* När du kör hello `ALTER USER` instruktion med hello `WITH LOGIN` alternativ, den måste vara hello endast instruktionen i en Transact-SQL-batch.
* för`CREATE/ALTER/DROP` en användare kräver hello `ALTER ANY USER` behörighet på hello-databasen.
* När en databasroll hello ägare försöker tooadd eller ta bort en annan databas användaren tooor från som databasrollen, hello följande fel kan uppstå: **användaren eller rollen 'Name' finns inte i den här databasen.** Detta fel uppstår eftersom hello användaren inte är synliga toohello ägare. tooresolve problemet kan bevilja hello rollen ägare hello `VIEW DEFINITION` behörighet på hello användare. 


## <a name="next-steps"></a>Nästa steg

- toolearn mer information om brandväggsregler, se [Azure SQL Database-brandvägg](sql-database-firewall-configure.md).
- En översikt över alla hello SQL Database-säkerhetsfunktioner finns [översikt över säkerheten i SQL](sql-database-security-overview.md).
- En självstudiekurs finns [skydda din Azure SQL Database](sql-database-security-tutorial.md).
- Information om vyer och lagrade procedurer finns i [Skapa vyer och lagrade procedurer](https://msdn.microsoft.com/library/ms365311.aspx)
- Information om hur du beviljar åtkomst tooa databasobjekt finns [bevilja åtkomst tooa databasobjekt](https://msdn.microsoft.com/library/ms365327.aspx)
