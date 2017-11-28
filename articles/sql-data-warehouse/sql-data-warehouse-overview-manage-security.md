---
title: Skydda en databas i SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 6ea45c40bc428282faf24b4a08f8b0d345adb3fd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="secure-a-database-in-sql-data-warehouse"></a><span data-ttu-id="b03f3-103">Skydda en databas i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="b03f3-103">Secure a database in SQL Data Warehouse</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b03f3-104">Översikt över säkerheten</span><span class="sxs-lookup"><span data-stu-id="b03f3-104">Security Overview</span></span>](sql-data-warehouse-overview-manage-security.md)
> * [<span data-ttu-id="b03f3-105">Autentisering</span><span class="sxs-lookup"><span data-stu-id="b03f3-105">Authentication</span></span>](sql-data-warehouse-authentication.md)
> * [<span data-ttu-id="b03f3-106">Kryptering (Portal)</span><span class="sxs-lookup"><span data-stu-id="b03f3-106">Encryption (Portal)</span></span>](sql-data-warehouse-encryption-tde.md)
> * [<span data-ttu-id="b03f3-107">Kryptering (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="b03f3-107">Encryption (T-SQL)</span></span>](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

<span data-ttu-id="b03f3-108">Den här artikeln beskriver hur grunderna för att skydda din Azure SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="b03f3-108">This article walks through the basics of securing your Azure SQL Data Warehouse database.</span></span> <span data-ttu-id="b03f3-109">I synnerhet i den här artikeln hjälper dig att komma igång med resurser för att begränsa åtkomst, skyddar data och övervakar aktiviteter på en databas.</span><span class="sxs-lookup"><span data-stu-id="b03f3-109">In particular, this article will get you started with resources for limiting access, protecting data, and monitoring activities on a database.</span></span>

## <a name="connection-security"></a><span data-ttu-id="b03f3-110">Anslutningssäkerhet</span><span class="sxs-lookup"><span data-stu-id="b03f3-110">Connection security</span></span>
<span data-ttu-id="b03f3-111">Anslutningssäkerhet avser hur du begränsar och säkrar anslutningar till databasen med hjälp av brandväggsregler och krypterad anslutning.</span><span class="sxs-lookup"><span data-stu-id="b03f3-111">Connection Security refers to how you restrict and secure connections to your database using firewall rules and connection encryption.</span></span>

<span data-ttu-id="b03f3-112">Brandväggsregler används av både servern och databasen för att avvisa anslutningsförsök från IP-adresser som inte uttryckligen finns på listan över godkända.</span><span class="sxs-lookup"><span data-stu-id="b03f3-112">Firewall rules are used by both the server and the database to reject connection attempts from IP addresses that have not been explicitly whitelisted.</span></span> <span data-ttu-id="b03f3-113">Om du vill tillåta anslutningar från ditt program eller klienten datorns offentliga IP-adress, måste du först skapa en brandväggsregel på servernivå med hjälp av Azure Portal, REST-API och PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b03f3-113">To allow connections from your application or client machine's public IP address, you must first create a server-level firewall rule using the Azure Portal, REST API, or PowerShell.</span></span> <span data-ttu-id="b03f3-114">Ett bra tips är att du begränsar de IP-adressintervall som tillåts via serverbrandväggen så mycket som möjligt.</span><span class="sxs-lookup"><span data-stu-id="b03f3-114">As a best practice, you should restrict the IP address ranges allowed through your server firewall as much as possible.</span></span>  <span data-ttu-id="b03f3-115">Kontrollera att brandväggen på ditt nätverk och en lokal dator tillåter utgående kommunikation på TCP-port 1433 för att komma åt Azure SQL Data Warehouse från den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="b03f3-115">To access Azure SQL Data Warehouse from your local computer, ensure the firewall on your network and local computer allows outgoing communication on TCP port 1433.</span></span>  <span data-ttu-id="b03f3-116">Mer information finns i [Azure SQL Database-brandvägg][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule], och [sp_set_database_ firewall_rule][sp_set_database_firewall_rule].</span><span class="sxs-lookup"><span data-stu-id="b03f3-116">For more information, see [Azure SQL Database firewall][Azure SQL Database firewall], [sp_set_firewall_rule][sp_set_firewall_rule], and [sp_set_database_firewall_rule][sp_set_database_firewall_rule].</span></span>

<span data-ttu-id="b03f3-117">Anslutningar till SQL Data Warehouse krypterad som standard.</span><span class="sxs-lookup"><span data-stu-id="b03f3-117">Connections to your SQL Data Warehouse are encrypted by default.</span></span>  <span data-ttu-id="b03f3-118">Ändra anslutningsinställningar för att inaktivera kryptering ignoreras.</span><span class="sxs-lookup"><span data-stu-id="b03f3-118">Modifying connection settings to disable encryption are ignored.</span></span>

## <a name="authentication"></a><span data-ttu-id="b03f3-119">Autentisering</span><span class="sxs-lookup"><span data-stu-id="b03f3-119">Authentication</span></span>
<span data-ttu-id="b03f3-120">Autentisering refererar till hur du styrkt din identitet vid anslutning till databasen.</span><span class="sxs-lookup"><span data-stu-id="b03f3-120">Authentication refers to how you prove your identity when connecting to the database.</span></span> <span data-ttu-id="b03f3-121">SQL Data Warehouse stöder för närvarande SQL Server-autentisering med ett användarnamn och lösenord som en Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b03f3-121">SQL Data Warehouse currently supports SQL Server Authentication with a username and password as well as a Azure Active Directory.</span></span> 

<span data-ttu-id="b03f3-122">När du skapade den logiska servern för databasen angav du en "serveradministratörsinloggning” med ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="b03f3-122">When you created the logical server for your database, you specified a "server admin" login with a username and password.</span></span> <span data-ttu-id="b03f3-123">Med dessa autentiseringsuppgifter, kan du autentisera till en databas på den servern som databasens ägare eller ”dbo” via SQL Server-autentisering.</span><span class="sxs-lookup"><span data-stu-id="b03f3-123">Using these credentials, you can authenticate to any database on that server as the database owner, or "dbo" through SQL Server Authentication.</span></span>

<span data-ttu-id="b03f3-124">Dock som bästa praxis bör bör användare i organisationen använda ett annat konto för autentisering.</span><span class="sxs-lookup"><span data-stu-id="b03f3-124">However, as a best practice, your organization’s users should use a different account to authenticate.</span></span> <span data-ttu-id="b03f3-125">Det här sättet kan du begränsa behörigheterna för programmet och minska riskerna med skadliga aktiviteter om din programkod är utsatt för en attack med SQL injection.</span><span class="sxs-lookup"><span data-stu-id="b03f3-125">This way you can limit the permissions granted to the application and reduce the risks of malicious activity in case your application code is vulnerable to a SQL injection attack.</span></span> 

<span data-ttu-id="b03f3-126">Om du vill skapa en SQL Server-autentiserad användare ansluta till den **master** databasen på servern med din inloggning för serveradministratör och skapa en ny server-inloggning.</span><span class="sxs-lookup"><span data-stu-id="b03f3-126">To create a SQL Server Authenticated user, connect to the **master** database on your server with your server admin login and create a new server login.</span></span>  <span data-ttu-id="b03f3-127">Dessutom är det en bra idé att skapa en användare i master-databasen för Azure SQL Data Warehouse-användare.</span><span class="sxs-lookup"><span data-stu-id="b03f3-127">Additionally, it is a good idea to create a user in the master database for Azure SQL Data Warehouse users.</span></span> <span data-ttu-id="b03f3-128">Skapa en användare i master tillåter en användare att logga in med verktyg som SSMS utan att ange ett databasnamn.</span><span class="sxs-lookup"><span data-stu-id="b03f3-128">Creating a user in master allows a user to login using tools like SSMS without specifying a database name.</span></span>  <span data-ttu-id="b03f3-129">Det gör också att de använder object explorer för att visa alla databaser på en SQLServer.</span><span class="sxs-lookup"><span data-stu-id="b03f3-129">It also allows them to use the object explorer to view all databases on a SQL server.</span></span>

```sql
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'Str0ng_password';
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

<span data-ttu-id="b03f3-130">Anslut till din **SQL Data Warehouse-databas** med din inloggning för serveradministratör och skapa en databasanvändare baserat på server-inloggning som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="b03f3-130">Then, connect to your **SQL Data Warehouse database** with your server admin login and create a database user based on the server login you just created.</span></span>

```sql
-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

<span data-ttu-id="b03f3-131">Om en användare kommer att göra ytterligare åtgärder som till exempel skapa inloggningar eller skapa nya databaser, måste de också tilldelas det `Loginmanager` och `dbmanager` roller i master-databasen.</span><span class="sxs-lookup"><span data-stu-id="b03f3-131">If a user will be doing additional operations such as creating logins or creating new databases, they will also need to be assigned to the `Loginmanager` and `dbmanager` roles in the master database.</span></span> <span data-ttu-id="b03f3-132">Mer information om dessa ytterligare roller och autentisera till en SQL-databas finns [hantera databaser och inloggningar i Azure SQL Database][Managing databases and logins in Azure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="b03f3-132">For more information on these additonal roles and authenticating to a SQL Database, see [Managing databases and logins in Azure SQL Database][Managing databases and logins in Azure SQL Database].</span></span>  <span data-ttu-id="b03f3-133">Mer information om Azure AD för SQL Data Warehouse finns [ansluter till SQL Data Warehouse med hjälp av Azure Active Directory Authentication][Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication].</span><span class="sxs-lookup"><span data-stu-id="b03f3-133">For more details on Azure AD for SQL Data Warehouse, see [Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication][Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication].</span></span>

## <a name="authorization"></a><span data-ttu-id="b03f3-134">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="b03f3-134">Authorization</span></span>
<span data-ttu-id="b03f3-135">Auktorisering hänvisar till vad du kan göra i en Azure SQL Data Warehouse-databas och detta styrs av ditt användarkonto rollmedlemskap och behörigheter.</span><span class="sxs-lookup"><span data-stu-id="b03f3-135">Authorization refers to what you can do within an Azure SQL Data Warehouse database, and this is controlled by your user account's role memberships and permissions.</span></span> <span data-ttu-id="b03f3-136">Ett bra tips är att du ska ge användare så få behörigheter som möjligt.</span><span class="sxs-lookup"><span data-stu-id="b03f3-136">As a best practice, you should grant users the least privileges necessary.</span></span> <span data-ttu-id="b03f3-137">Azure SQL Data Warehouse gör det enkelt att hantera roller i T-SQL:</span><span class="sxs-lookup"><span data-stu-id="b03f3-137">Azure SQL Data Warehouse makes this easy to manage with roles in T-SQL:</span></span>

```sql
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

<span data-ttu-id="b03f3-138">Serveradministratörskontot som du ansluter med är medlem i db_owner som har behörighet att göra vad som helst i databasen.</span><span class="sxs-lookup"><span data-stu-id="b03f3-138">The server admin account you are connecting with is a member of db_owner, which has authority to do anything within the database.</span></span> <span data-ttu-id="b03f3-139">Spara det här kontot för att distribuera schemauppgraderingar och andra hanteringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="b03f3-139">Save this account for deploying schema upgrades and other management operations.</span></span> <span data-ttu-id="b03f3-140">Använd kontot "ApplicationUser" med mer begränsade behörigheter för att ansluta från ditt program till databasen med den minsta behörigheten som krävs av programmet.</span><span class="sxs-lookup"><span data-stu-id="b03f3-140">Use the "ApplicationUser" account with more limited permissions to connect from your application to the database with the least privileges needed by your application.</span></span>

<span data-ttu-id="b03f3-141">Det finns olika sätt att ytterligare begränsa vad en användare kan göra med Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="b03f3-141">There are ways to further limit what a user can do with Azure SQL Database:</span></span>

* <span data-ttu-id="b03f3-142">Detaljerade [behörigheter] [ Permissions] kan du kontrollen vilka åtgärder som du kan på enskilda kolumner, tabeller, vyer, procedurer och andra objekt i databasen.</span><span class="sxs-lookup"><span data-stu-id="b03f3-142">Granular [Permissions][Permissions] let you control which operations you can do on individual columns, tables, views, procedures, and other objects in the database.</span></span> <span data-ttu-id="b03f3-143">Använda detaljerade behörigheter har störst kontroll och bevilja lägsta behörigheten som krävs.</span><span class="sxs-lookup"><span data-stu-id="b03f3-143">Use granular permissions to have the most control and grant the minimum permissions necessary.</span></span> <span data-ttu-id="b03f3-144">Detaljerad behörighet system är ganska komplex och kräver vissa undersökning du använder effektivt.</span><span class="sxs-lookup"><span data-stu-id="b03f3-144">The granular permission system is somewhat complicated and will require some study to use effectively.</span></span>
* <span data-ttu-id="b03f3-145">[Databasrollerna] [ Database roles] än db_datareader och db_datawriter kan användas för att skapa mer kraftfulla program användarkonton eller mindre kraftfulla hanteringskonton.</span><span class="sxs-lookup"><span data-stu-id="b03f3-145">[Database roles][Database roles] other than db_datareader and db_datawriter can be used to create more powerful application user accounts or less powerful management accounts.</span></span> <span data-ttu-id="b03f3-146">Inbyggda fasta databasroller ger ett enkelt sätt att bevilja behörighet, men kan leda till att bevilja fler behörigheter än nödvändigt.</span><span class="sxs-lookup"><span data-stu-id="b03f3-146">The built-in fixed database roles provide an easy way to grant permissions, but can result in granting more permissions than are necessary.</span></span>
* <span data-ttu-id="b03f3-147">[Lagrade procedurer] [ Stored procedures] kan användas för att begränsa de åtgärder som kan göras på databasen.</span><span class="sxs-lookup"><span data-stu-id="b03f3-147">[Stored procedures][Stored procedures] can be used to limit the actions that can be taken on the database.</span></span>

<span data-ttu-id="b03f3-148">Att hantera databaser och logiska servrar från den klassiska Azure-portalen eller via Azure Resource Manager API styrs av portalanvändarkontots rolltilldelningar.</span><span class="sxs-lookup"><span data-stu-id="b03f3-148">Managing databases and logical servers from the Azure Classic Portal or using the Azure Resource Manager API is controlled by your portal user account's role assignments.</span></span> <span data-ttu-id="b03f3-149">Mer information om det här avsnittet finns [rollbaserad åtkomstkontroll i Azure Portal][Role-based access control in Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="b03f3-149">For more information on this topic, see [Role-based access control in Azure Portal][Role-based access control in Azure Portal].</span></span>

## <a name="encryption"></a><span data-ttu-id="b03f3-150">Kryptering</span><span class="sxs-lookup"><span data-stu-id="b03f3-150">Encryption</span></span>
<span data-ttu-id="b03f3-151">Azure SQL Data Warehouse Transparent Data kryptering (TDE) skyddar mot hot från skadlig aktivitet genom att utföra realtid kryptering och dekryptering av data i vila.</span><span class="sxs-lookup"><span data-stu-id="b03f3-151">Azure SQL Data Warehouse Transparent Data Encryption (TDE) helps protect against the threat of malicious activity by performing real-time encryption and decryption of your data at rest.</span></span>  <span data-ttu-id="b03f3-152">När du krypterar din databas, krypteras tillhörande säkerhetskopior och transaktionsloggfiler utan ändringar i dina program.</span><span class="sxs-lookup"><span data-stu-id="b03f3-152">When you encrypt your database, associated backups and transaction log files are encrypted without requiring any changes to your applications.</span></span> <span data-ttu-id="b03f3-153">TDE krypterar lagring av en hel databas med hjälp av en symmetrisk nyckel som heter databaskrypteringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="b03f3-153">TDE encrypts the storage of an entire database by using a symmetric key called the database encryption key.</span></span> <span data-ttu-id="b03f3-154">Databaskrypteringsnyckeln skyddas av en inbyggd servercertifikat i SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="b03f3-154">In SQL Database the database encryption key is protected by a built-in server certificate.</span></span> <span data-ttu-id="b03f3-155">Inbyggda certifikatet är unikt för varje SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="b03f3-155">The built-in server certificate is unique for each SQL Database server.</span></span> <span data-ttu-id="b03f3-156">Microsoft roteras automatiskt dessa certifikat minst var 90: e dag.</span><span class="sxs-lookup"><span data-stu-id="b03f3-156">Microsoft automatically rotates these certificates at least every 90 days.</span></span> <span data-ttu-id="b03f3-157">Krypteringsalgoritmen som används av SQL Data Warehouse är AES-256.</span><span class="sxs-lookup"><span data-stu-id="b03f3-157">The encryption algorithm used by SQL Data Warehouse is AES-256.</span></span> <span data-ttu-id="b03f3-158">En allmän beskrivning av TDE finns [Transparent datakryptering][Transparent Data Encryption].</span><span class="sxs-lookup"><span data-stu-id="b03f3-158">For a general description of TDE, see [Transparent Data Encryption][Transparent Data Encryption].</span></span>

<span data-ttu-id="b03f3-159">Du kan kryptera din databas med hjälp av den [Azure Portal] [ Encryption with Portal] eller [T-SQL][Encryption with TSQL].</span><span class="sxs-lookup"><span data-stu-id="b03f3-159">You can encrypt your database using the [Azure Portal][Encryption with Portal] or [T-SQL][Encryption with TSQL].</span></span>

## <a name="next-steps"></a><span data-ttu-id="b03f3-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b03f3-160">Next steps</span></span>
<span data-ttu-id="b03f3-161">Information och exempel på hur du ansluter till ditt SQL Data Warehouse med olika protokoll finns [Anslut till SQL Data Warehouse][Connect to SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="b03f3-161">For details and examples on connecting to your SQL Data Warehouse with different protocols, see [Connect to SQL Data Warehouse][Connect to SQL Data Warehouse].</span></span>

<!--Image references-->

<!--Article references-->
[Connect to SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Encryption with Portal]: ./sql-data-warehouse-encryption-tde.md
[Encryption with TSQL]: ./sql-data-warehouse-encryption-tde-tsql.md
[Connecting to SQL Data Warehouse By Using Azure Active Directory Authentication]: ./sql-data-warehouse-authentication.md

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
