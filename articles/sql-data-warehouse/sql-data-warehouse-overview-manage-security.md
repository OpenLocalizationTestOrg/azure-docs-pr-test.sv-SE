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
# <a name="secure-a-database-in-sql-data-warehouse"></a><span data-ttu-id="f62b6-103">Skydda en databas i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f62b6-103">Secure a database in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f62b6-104">Översikt över säkerheten</span><span class="sxs-lookup"><span data-stu-id="f62b6-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="f62b6-105">Autentisering</span><span class="sxs-lookup"><span data-stu-id="f62b6-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="f62b6-106">Kryptering (Portal)</span><span class="sxs-lookup"><span data-stu-id="f62b6-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="f62b6-107">Kryptering (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="f62b6-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

<span data-ttu-id="f62b6-108">Den här artikeln beskriver hur hello grunderna för att skydda din Azure SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="f62b6-108">This article walks through hello basics of securing your Azure SQL Data Warehouse database.</span></span> <span data-ttu-id="f62b6-109">I synnerhet i den här artikeln hjälper dig att komma igång med resurser för att begränsa åtkomst, skyddar data och övervakar aktiviteter på en databas.</span><span class="sxs-lookup"><span data-stu-id="f62b6-109">In particular, this article will get you started with resources for limiting access, protecting data, and monitoring activities on a database.</span></span>

## <a name="connection-security"></a><span data-ttu-id="f62b6-110">Anslutningssäkerhet</span><span class="sxs-lookup"><span data-stu-id="f62b6-110">Connection security</span></span>
<span data-ttu-id="f62b6-111">Anslutningssäkerhet refererar toohow du begränsa och skydda anslutningar tooyour databas med hjälp av brandväggsregler och krypterad anslutning.</span><span class="sxs-lookup"><span data-stu-id="f62b6-111">Connection Security refers toohow you restrict and secure connections tooyour database using firewall rules and connection encryption.</span></span>

<span data-ttu-id="f62b6-112">Brandväggsregler som används av både hello server och hello databasen tooreject anslutningsförsök från IP-adresser som inte har uttryckligen godkända.</span><span class="sxs-lookup"><span data-stu-id="f62b6-112">Firewall rules are used by both hello server and hello database tooreject connection attempts from IP addresses that have not been explicitly whitelisted.</span></span> <span data-ttu-id="f62b6-113">tooallow anslutningar från ditt program eller klienten datorns offentliga IP-adress, måste du först skapa en brandväggsregel på servernivå med hjälp av hello Azure-portalen, REST-API och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f62b6-113">tooallow connections from your application or client machine's public IP address, you must first create a server-level firewall rule using hello Azure Portal, REST API, or PowerShell.</span></span> <span data-ttu-id="f62b6-114">Som bästa praxis bör du begränsa hello IP-adressintervall tillåts via brandväggen server så mycket som möjligt.</span><span class="sxs-lookup"><span data-stu-id="f62b6-114">As a best practice, you should restrict hello IP address ranges allowed through your server firewall as much as possible.</span></span>  <span data-ttu-id="f62b6-115">tooaccess Azure SQL Data Warehouse från din lokala dator, kontrollera hello brandväggen på ditt nätverk och en lokal dator tillåter utgående kommunikation på TCP-port 1433.</span><span class="sxs-lookup"><span data-stu-id="f62b6-115">tooaccess Azure SQL Data Warehouse from your local computer, ensure hello firewall on your network and local computer allows outgoing communication on TCP port 1433.</span></span>  <span data-ttu-id="f62b6-116">Mer information finns i [Azure SQL Database-brandvägg][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule], och [sp_set_database_ firewall_rule][sp_set_database_firewall_rule].</span><span class="sxs-lookup"><span data-stu-id="f62b6-116">For more information, see [Azure SQL Database firewall][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule], and [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span></span>

<span data-ttu-id="f62b6-117">Anslutningar tooyour SQL Data Warehouse krypterad som standard.</span><span class="sxs-lookup"><span data-stu-id="f62b6-117">Connections tooyour SQL Data Warehouse are encrypted by default.</span></span>  <span data-ttu-id="f62b6-118">Ändra anslutning inställningar toodisable kryptering ignoreras.</span><span class="sxs-lookup"><span data-stu-id="f62b6-118">Modifying connection settings toodisable encryption are ignored.</span></span>

## <a name="authentication"></a><span data-ttu-id="f62b6-119">Autentisering</span><span class="sxs-lookup"><span data-stu-id="f62b6-119">Authentication</span></span>
<span data-ttu-id="f62b6-120">Autentisering refererar toohow du styrka din identitet när du ansluter toohello databas.</span><span class="sxs-lookup"><span data-stu-id="f62b6-120">Authentication refers toohow you prove your identity when connecting toohello database.</span></span> <span data-ttu-id="f62b6-121">SQL Data Warehouse stöder för närvarande SQL Server-autentisering med ett användarnamn och lösenord som en Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="f62b6-121">SQL Data Warehouse currently supports SQL Server Authentication with a username and password as well as a Azure Active Directory.</span></span> 

<span data-ttu-id="f62b6-122">När du skapade hello logisk server för din databas, anges en ”serveradministratören” inloggning med ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="f62b6-122">When you created hello logical server for your database, you specified a "server admin" login with a username and password.</span></span> <span data-ttu-id="f62b6-123">Du kan autentisera tooany databasen på den servern som hello databasens ägare eller ”dbo” via SQL Server-autentisering med dessa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f62b6-123">Using these credentials, you can authenticate tooany database on that server as hello database owner, or "dbo" through SQL Server Authentication.</span></span>

<span data-ttu-id="f62b6-124">Användare i organisationen bör dock som bästa praxis använda ett annat konto tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="f62b6-124">However, as a best practice, your organization’s users should use a different account tooauthenticate.</span></span> <span data-ttu-id="f62b6-125">Det här sättet kan du begränsa behörigheterna för hello toohello program och minska hello risker skadlig programvara om din programkod är sårbar tooa SQL injection attack.</span><span class="sxs-lookup"><span data-stu-id="f62b6-125">This way you can limit hello permissions granted toohello application and reduce hello risks of malicious activity in case your application code is vulnerable tooa SQL injection attack.</span></span> 

<span data-ttu-id="f62b6-126">toocreate SQL Server-autentiserad användare ansluta toohello **master** databasen på servern med din inloggning för serveradministratör och skapa en ny server-inloggning.</span><span class="sxs-lookup"><span data-stu-id="f62b6-126">toocreate a SQL Server Authenticated user, connect toohello **master** database on your server with your server admin login and create a new server login.</span></span>  <span data-ttu-id="f62b6-127">Dessutom är det en bra idé toocreate en användare i hello huvuddatabasen för Azure SQL Data Warehouse-användare.</span><span class="sxs-lookup"><span data-stu-id="f62b6-127">Additionally, it is a good idea toocreate a user in hello master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="f62b6-128">Skapa en användare i master kan en användare toologin med hjälp av verktyg som SSMS utan att ange ett databasnamn.</span><span class="sxs-lookup"><span data-stu-id="f62b6-128">Creating a user in master allows a user toologin using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="f62b6-129">Det gör dem också toouse hello object explorer tooview alla databaser på en SQLServer.</span><span class="sxs-lookup"><span data-stu-id="f62b6-129">It also allows them toouse hello object explorer tooview all databases on a SQL server.</span></span>

```sql
-- Connect toomaster database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

<span data-ttu-id="f62b6-130">Anslut tooyour **SQL Data Warehouse-databas** med din inloggning för serveradministratör och skapa en databasanvändare baserat på hello server-inloggning som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="f62b6-130">Then, connect tooyour **SQL Data Warehouse database** with your server admin login and create a database user based on hello server login you just created.</span></span>

```sql
-- Connect tooSQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

<span data-ttu-id="f62b6-131">Om en användare kommer att göra ytterligare åtgärder som till exempel skapa inloggningar eller skapa nya databaser, måste de också toobe tilldelade toohello `Loginmanager` och `dbmanager` roller i hello master-databasen.</span><span class="sxs-lookup"><span data-stu-id="f62b6-131">If a user will be doing additional operations such as creating logins or creating new databases, they will also need toobe assigned toohello `Loginmanager` and `dbmanager` roles in hello master database.</span></span> <span data-ttu-id="f62b6-132">Mer information om dessa ytterligare roller och autentiserande tooa SQL-databas finns [hantera databaser och inloggningar i Azure SQL Database][Managing databases and logins in Azure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="f62b6-132">For more information on these additonal roles and authenticating tooa SQL Database, see [Managing databases and logins in Azure SQL Database][Managing databases and logins in Azure SQL Database].</span></span>  <span data-ttu-id="f62b6-133">Mer information om Azure AD för SQL Data Warehouse finns [ansluter tooSQL Data Warehouse med hjälp av Azure Active Directory Authentication][Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication].</span><span class="sxs-lookup"><span data-stu-id="f62b6-133">For more details on Azure AD for SQL Data Warehouse, see [Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication][Connecting tooSQL Data Warehouse By Using Azure Active Directory Authentication].</span></span>

## <a name="authorization"></a><span data-ttu-id="f62b6-134">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="f62b6-134">Authorization</span></span>
<span data-ttu-id="f62b6-135">Auktorisering refererar toowhat kan du göra i en Azure SQL Data Warehouse-databas och detta styrs av ditt användarkonto rollmedlemskap och behörigheter.</span><span class="sxs-lookup"><span data-stu-id="f62b6-135">Authorization refers toowhat you can do within an Azure SQL Data Warehouse database, and this is controlled by your user account's role memberships and permissions.</span></span> <span data-ttu-id="f62b6-136">Som bästa praxis bör du bevilja användare hello lägsta behörighet.</span><span class="sxs-lookup"><span data-stu-id="f62b6-136">As a best practice, you should grant users hello least privileges necessary.</span></span> <span data-ttu-id="f62b6-137">Azure SQL Data Warehouse gör den här enkelt toomanage med roller i T-SQL:</span><span class="sxs-lookup"><span data-stu-id="f62b6-137">Azure SQL Data Warehouse makes this easy toomanage with roles in T-SQL:</span></span>

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser tooread data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser toowrite data
```

<span data-ttu-id="f62b6-138">administratörskonto för hello servern du ansluter med är medlem i db_owner som har myndigheten toodo något hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="f62b6-138">hello server admin account you are connecting with is a member of db_owner, which has authority toodo anything within hello database.</span></span> <span data-ttu-id="f62b6-139">Spara det här kontot för att distribuera schemauppgraderingar och andra hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="f62b6-139">Save this account for deploying schema upgrades and other management operations.</span></span> <span data-ttu-id="f62b6-140">Använda hello ”ApplicationUser” konto med mer begränsade behörigheter tooconnect från programmet toohello databasen med hello lägsta privilegier som behövs i programmet.</span><span class="sxs-lookup"><span data-stu-id="f62b6-140">Use hello "ApplicationUser" account with more limited permissions tooconnect from your application toohello database with hello least privileges needed by your application.</span></span>

<span data-ttu-id="f62b6-141">Det finns olika sätt toofurther gränsen vad en användare kan göra med Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="f62b6-141">There are ways toofurther limit what a user can do with Azure SQL Database:</span></span>

* <span data-ttu-id="f62b6-142">Detaljerade [behörigheter] [ Permissions] kan du kontrollen vilka åtgärder som du kan på enskilda kolumner, tabeller, vyer, procedurer och andra objekt i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="f62b6-142">Granular [Permissions][Permissions] let you control which operations you can do on individual columns, tables, views, procedures, and other objects in hello database.</span></span> <span data-ttu-id="f62b6-143">Använd detaljerade behörigheter toohave hello mest kontroll och ge hello minsta möjliga behörigheter krävs.</span><span class="sxs-lookup"><span data-stu-id="f62b6-143">Use granular permissions toohave hello most control and grant hello minimum permissions necessary.</span></span> <span data-ttu-id="f62b6-144">hello detaljerad behörighet system är ganska komplex och kräver vissa undersökning toouse effektivt.</span><span class="sxs-lookup"><span data-stu-id="f62b6-144">hello granular permission system is somewhat complicated and will require some study toouse effectively.</span></span>
* <span data-ttu-id="f62b6-145">[Databasrollerna] [ Database roles] utöver db_datareader och db_datawriter kan använda toocreate mer kraftfulla program användarkonton eller mindre kraftfulla hanteringskonton.</span><span class="sxs-lookup"><span data-stu-id="f62b6-145">[Database roles][Database roles] other than db_datareader and db_datawriter can be used toocreate more powerful application user accounts or less powerful management accounts.</span></span> <span data-ttu-id="f62b6-146">hello inbyggda fasta databasroller ger ett enkelt sätt toogrant behörigheter, men kan resultera i att bevilja fler behörigheter än nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="f62b6-146">hello built-in fixed database roles provide an easy way toogrant permissions, but can result in granting more permissions than are necessary.</span></span>
* <span data-ttu-id="f62b6-147">[Lagrade procedurer] [ Stored procedures] kan vara används toolimit hello-åtgärder som kan göras på hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="f62b6-147">[Stored procedures][Stored procedures] can be used toolimit hello actions that can be taken on hello database.</span></span>

<span data-ttu-id="f62b6-148">Hantera databaser och logiska servrar från hello klassiska Azure-portalen eller med hjälp av hello Azure Resource Manager API styrs av din portal användarkonto rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="f62b6-148">Managing databases and logical servers from hello Azure Classic Portal or using hello Azure Resource Manager API is controlled by your portal user account's role assignments.</span></span> <span data-ttu-id="f62b6-149">Mer information om det här avsnittet finns [rollbaserad åtkomstkontroll i Azure Portal][Role-based access control in Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="f62b6-149">For more information on this topic, see [Role-based access control in Azure Portal][Role-based access control in Azure Portal].</span></span>

## <a name="encryption"></a><span data-ttu-id="f62b6-150">Kryptering</span><span class="sxs-lookup"><span data-stu-id="f62b6-150">Encryption</span></span>
<span data-ttu-id="f62b6-151">Azure SQL Data Warehouse Transparent Data kryptering (TDE) skyddar mot hello hotet att skadlig aktivitet genom att utföra realtid kryptering och dekryptering av data i vila.</span><span class="sxs-lookup"><span data-stu-id="f62b6-151">Azure SQL Data Warehouse Transparent Data Encryption (TDE) helps protect against hello threat of malicious activity by performing real-time encryption and decryption of your data at rest.</span></span>  <span data-ttu-id="f62b6-152">När du krypterar din databas, krypteras tillhörande säkerhetskopior och transaktionsloggfiler utan några ändringar tooyour program.</span><span class="sxs-lookup"><span data-stu-id="f62b6-152">When you encrypt your database, associated backups and transaction log files are encrypted without requiring any changes tooyour applications.</span></span> <span data-ttu-id="f62b6-153">TDE krypterar hello lagring av en hel databas med en symmetrisk nyckel kallas hello krypteringsnyckeln för databasen.</span><span class="sxs-lookup"><span data-stu-id="f62b6-153">TDE encrypts hello storage of an entire database by using a symmetric key called hello database encryption key.</span></span> <span data-ttu-id="f62b6-154">I SQL-databas skyddas hello databaskrypteringsnyckeln av inbyggda servercertifikat.</span><span class="sxs-lookup"><span data-stu-id="f62b6-154">In SQL Database hello database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="f62b6-155">hello inbyggda servercertifikatet är unikt för varje SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="f62b6-155">hello built-in server certificate is unique for each SQL Database server.</span></span> <span data-ttu-id="f62b6-156">Microsoft roteras automatiskt dessa certifikat minst var 90: e dag.</span><span class="sxs-lookup"><span data-stu-id="f62b6-156">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="f62b6-157">hello krypteringsalgoritm som används av SQL Data Warehouse är AES-256.</span><span class="sxs-lookup"><span data-stu-id="f62b6-157">hello encryption algorithm used by SQL Data Warehouse is AES-256.</span></span> <span data-ttu-id="f62b6-158">En allmän beskrivning av TDE finns [Transparent datakryptering][Transparent Data Encryption].</span><span class="sxs-lookup"><span data-stu-id="f62b6-158">For a general description of TDE, see [Transparent Data Encryption][Transparent Data Encryption].</span></span>

<span data-ttu-id="f62b6-159">Du kan kryptera din databas med hjälp av hello [Azure Portal] [ Encryption with Portal] eller [T-SQL][Encryption with TSQL].</span><span class="sxs-lookup"><span data-stu-id="f62b6-159">You can encrypt your database using hello [Azure Portal][Encryption with Portal] or [T-SQL][Encryption with TSQL].</span></span>

## <a name="next-steps"></a><span data-ttu-id="f62b6-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f62b6-160">Next steps</span></span>
<span data-ttu-id="f62b6-161">Information och exempel på hur du ansluter tooyour SQL Data Warehouse med olika protokoll finns [ansluta tooSQL datalagret][Connect tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="f62b6-161">For details and examples on connecting tooyour SQL Data Warehouse with different protocols, see [Connect tooSQL Data Warehouse][Connect tooSQL Data Warehouse].</span></span>

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
