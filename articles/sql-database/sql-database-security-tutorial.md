---
title: aaaSecure Azure SQL database | Microsoft Docs
description: Mer information om tekniker och funktioner toosecure Azure SQL database.
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: 1450d633d6f65faf1b8a2dc0dc7dfe996fb0719d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-azure-sql-database"></a><span data-ttu-id="72834-103">Skydda din Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="72834-103">Secure your Azure SQL Database</span></span>

<span data-ttu-id="72834-104">SQL-databas skyddar dina data genom att begränsa åtkomst tooyour databasen med hjälp av brandväggsregler, autentiseringsmekanismer som kräver användare tooprove deras identitet och auktorisering toodata genom rollbaserad medlemskap och behörigheter, samt via säkerhet på radnivå och dynamisk datamaskning.</span><span class="sxs-lookup"><span data-stu-id="72834-104">SQL Database secures your data by limiting access tooyour database using firewall rules, authentication mechanisms requiring users tooprove their identity, and authorization toodata through role-based memberships and permissions, as well as through row-level security and dynamic data masking.</span></span>

<span data-ttu-id="72834-105">Du kan förbättra hello skyddet av databasen mot obehöriga användare eller obehörig åtkomst med bara några få enkla steg.</span><span class="sxs-lookup"><span data-stu-id="72834-105">You can improve hello protection of your database against malicious users or unauthorized access with just a few simple steps.</span></span> <span data-ttu-id="72834-106">I den här kursen lär du dig:</span><span class="sxs-lookup"><span data-stu-id="72834-106">In this tutorial you learn to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="72834-107">Konfigurera brandväggsregler på servernivå för servern i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="72834-107">Set up server-level firewall rules for your server in hello Azure portal</span></span>
> * <span data-ttu-id="72834-108">Konfigurera brandväggsregler på databasnivå för databasen med hjälp av SSMS</span><span class="sxs-lookup"><span data-stu-id="72834-108">Set up database-level firewall rules for your database using SSMS</span></span>
> * <span data-ttu-id="72834-109">Ansluta tooyour databasen med hjälp av en säker anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="72834-109">Connect tooyour database using a secure connection string</span></span>
> * <span data-ttu-id="72834-110">Hantera användarnas åtkomst</span><span class="sxs-lookup"><span data-stu-id="72834-110">Manage user access</span></span>
> * <span data-ttu-id="72834-111">Skydda dina data med kryptering</span><span class="sxs-lookup"><span data-stu-id="72834-111">Protect your data with encryption</span></span>
> * <span data-ttu-id="72834-112">Aktivera granskning av SQL-databas</span><span class="sxs-lookup"><span data-stu-id="72834-112">Enable SQL Database auditing</span></span>
> * <span data-ttu-id="72834-113">Aktivera hotidentifiering för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="72834-113">Enable SQL Database threat detection</span></span>

<span data-ttu-id="72834-114">Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="72834-114">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="72834-115">Krav</span><span class="sxs-lookup"><span data-stu-id="72834-115">Prerequisites</span></span>

<span data-ttu-id="72834-116">toocomplete den här självstudiekursen, se till att du har hello följande:</span><span class="sxs-lookup"><span data-stu-id="72834-116">toocomplete this tutorial, make sure you have hello following:</span></span>

- <span data-ttu-id="72834-117">Installerade hello senaste versionen av [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="72834-117">Installed hello newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).</span></span> 
- <span data-ttu-id="72834-118">Installerade Microsoft Excel</span><span class="sxs-lookup"><span data-stu-id="72834-118">Installed Microsoft Excel</span></span>
- <span data-ttu-id="72834-119">Skapa en Azure SQL-server och databas - finns [skapa en Azure SQL database i hello Azure-portalen](sql-database-get-started-portal.md), [skapar en enda Azure SQL-databas med hello Azure CLI](sql-database-get-started-cli.md), och [skapa en enskild SQL Azure databasen med hjälp av PowerShell](sql-database-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="72834-119">Created an Azure SQL server and database - See [Create an Azure SQL database in hello Azure portal](sql-database-get-started-portal.md), [Create a single Azure SQL database using hello Azure CLI](sql-database-get-started-cli.md), and [Create a single Azure SQL database using PowerShell](sql-database-get-started-powershell.md).</span></span> 

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="72834-120">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="72834-120">Log in toohello Azure portal</span></span>

<span data-ttu-id="72834-121">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="72834-121">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="72834-122">Skapa en brandväggsregel på servernivå i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="72834-122">Create a server-level firewall rule in hello Azure portal</span></span>

<span data-ttu-id="72834-123">SQL-databaser är skyddade av en brandvägg i Azure.</span><span class="sxs-lookup"><span data-stu-id="72834-123">SQL databases are protected by a firewall in Azure.</span></span> <span data-ttu-id="72834-124">Som standard är alla anslutningar toohello server och hello databaser hello-servern avvisade förutom anslutningar från andra Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="72834-124">By default, all connections toohello server and hello databases inside hello server are rejected except for connections from other Azure services.</span></span> <span data-ttu-id="72834-125">Mer information finns i [Azure SQL Database server- och databasnivå brandväggsregler](sql-database-firewall-configure.md).</span><span class="sxs-lookup"><span data-stu-id="72834-125">For more information, see [Azure SQL Database server-level and database-level firewall rules](sql-database-firewall-configure.md).</span></span>

<span data-ttu-id="72834-126">hello säkraste konfigurationen är tooset 'Tillåter åtkomst tooAzure services' tooOFF.</span><span class="sxs-lookup"><span data-stu-id="72834-126">hello most secure configuration is tooset 'Allow access tooAzure services' tooOFF.</span></span> <span data-ttu-id="72834-127">Om du behöver tooconnect toohello databasen från en virtuell Azure-dator eller ett moln-tjänst kan du skapa en [reserverade IP: N](../virtual-network/virtual-networks-reserved-public-ip.md) och att bara tillåta hello reserverade IP-adress åtkomst via hello-brandväggen.</span><span class="sxs-lookup"><span data-stu-id="72834-127">If you need tooconnect toohello database from an Azure VM or cloud service, you should create a [Reserved IP](../virtual-network/virtual-networks-reserved-public-ip.md) and allow only hello reserved IP address access through hello firewall.</span></span> 

<span data-ttu-id="72834-128">Följ dessa steg toocreate en [SQL-databas brandväggsregel på servernivå](sql-database-firewall-configure.md) för server tooallow-anslutningar från en specifik IP-adress.</span><span class="sxs-lookup"><span data-stu-id="72834-128">Follow these steps toocreate a [SQL Database server-level firewall rule](sql-database-firewall-configure.md) for your server tooallow connections from a specific IP address.</span></span> 

> [!NOTE]
> <span data-ttu-id="72834-129">Om du har skapat en exempeldatabas i Azure med hjälp av en av hello tidigare självstudier eller Snabbstart och är utför den här kursen på en dator med hello samma IP-adress som den hade när du har gått igenom dessa självstudier, du kan hoppa över detta steg som du har redan skapat en brandväggsregel på servernivå.</span><span class="sxs-lookup"><span data-stu-id="72834-129">If you have created a sample database in Azure using one of hello previous tutorials or quickstarts and are performing this tutorial on a computer with hello same IP address that it had when you walked through those tutorials, you can skip this step as you will have already created a server-level firewall rule.</span></span>
>

1. <span data-ttu-id="72834-130">Klicka på **SQL-databaser** från hello vänstra menyn och klicka på hello databasen du vill tooconfigure hello brandväggsregeln på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="72834-130">Click **SQL databases** from hello left-hand menu and click hello database you would like tooconfigure hello firewall rule for on hello **SQL databases** page.</span></span> <span data-ttu-id="72834-131">hello översiktssidan för din databas öppnas som visar du hello fullständigt kvalificerade servernamnet (exempelvis **mynewserver 20170313.database.windows.net**) och innehåller alternativ för ytterligare konfiguration.</span><span class="sxs-lookup"><span data-stu-id="72834-131">hello overview page for your database opens, showing you hello fully qualified server name (such as **mynewserver-20170313.database.windows.net**) and provides options for further configuration.</span></span>

      ![brandväggsregler för server](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. <span data-ttu-id="72834-133">Klicka på **ange serverbrandvägg** hello verktygsfältet enligt hello föregående bild.</span><span class="sxs-lookup"><span data-stu-id="72834-133">Click **Set server firewall** on hello toolbar as shown in hello previous image.</span></span> <span data-ttu-id="72834-134">Hej **brandväggsinställningar** öppnas sidan för hello SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="72834-134">hello **Firewall settings** page for hello SQL Database server opens.</span></span> 

3. <span data-ttu-id="72834-135">Klicka på **lägga till klientens IP-Adressen** på hello verktygsfältet tooadd hello offentliga IP-adress hello datorn anslutna toohello portal med eller ange hello brandväggsregel manuellt och klickar sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="72834-135">Click **Add client IP** on hello toolbar tooadd hello public IP address of hello computer connected toohello portal with or enter hello firewall rule manually and then click **Save**.</span></span>

      ![ange brandväggsregel för server](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. <span data-ttu-id="72834-137">Klicka på **OK** och klicka sedan på hello **X** tooclose hello **brandväggsinställningar** sidan.</span><span class="sxs-lookup"><span data-stu-id="72834-137">Click **OK** and then click hello **X** tooclose hello **Firewall settings** page.</span></span>

<span data-ttu-id="72834-138">Nu kan du ansluta tooany databas i hello server med hello angivna IP-adress eller IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="72834-138">You can now connect tooany database in hello server with hello specified IP address or IP address range.</span></span>

> [!NOTE]
> <span data-ttu-id="72834-139">SQL Database kommunicerar via port 1433.</span><span class="sxs-lookup"><span data-stu-id="72834-139">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="72834-140">Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 1433 inte av ditt nätverks brandvägg.</span><span class="sxs-lookup"><span data-stu-id="72834-140">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="72834-141">I så fall, blir inte kan tooconnect tooyour Azure SQL Database-server om din IT-avdelning öppnar port 1433.</span><span class="sxs-lookup"><span data-stu-id="72834-141">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a><span data-ttu-id="72834-142">Skapa en brandväggsregel på databasnivå med hjälp av SSMS</span><span class="sxs-lookup"><span data-stu-id="72834-142">Create a database-level firewall rule using SSMS</span></span>

<span data-ttu-id="72834-143">Brandväggsregler på databasnivå aktivera toocreate olika brandväggsinställningar för olika databaser inom hello samma logiska server och toocreate brandväggsregler som kan användas-vilket innebär att de följer hello databas under en [växling vid fel ](sql-database-geo-replication-overview.md) i stället för att lagras på hello SQLServer.</span><span class="sxs-lookup"><span data-stu-id="72834-143">Database-level firewall rules enable you toocreate different firewall settings for different databases within hello same logical server and toocreate firewall rules that are portable - meaning that they follow hello database during a [failover](sql-database-geo-replication-overview.md) rather than being stored on hello SQL server.</span></span> <span data-ttu-id="72834-144">Brandväggsregler på databasnivå kan bara vara konfigurerad med hjälp av Transact-SQL-uttryck och bara när du har konfigurerat hello första servernivå brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="72834-144">Database-level firewall rules can only be configured by using Transact-SQL statements and only after you have configured hello first server-level firewall rule.</span></span> <span data-ttu-id="72834-145">Mer information finns i [Azure SQL Database server- och databasnivå brandväggsregler](sql-database-firewall-configure.md).</span><span class="sxs-lookup"><span data-stu-id="72834-145">For more information, see [Azure SQL Database server-level and database-level firewall rules](sql-database-firewall-configure.md).</span></span>

<span data-ttu-id="72834-146">Följer dessa steg toocreate en databasspecifika brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="72834-146">Follows these steps toocreate a database-specific firewall rule.</span></span>

1. <span data-ttu-id="72834-147">Ansluta tooyour databasen, till exempel med [SQL Server Management Studio](./sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="72834-147">Connect tooyour database, for example using [SQL Server Management Studio](./sql-database-connect-query-ssms.md).</span></span>

2. <span data-ttu-id="72834-148">Högerklicka på hello-databas som du vill tooadd en brandvägg regel för och klicka på i Object Explorer **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="72834-148">In Object Explorer, right-click on hello database you want tooadd a firewall rule for and click **New Query**.</span></span> <span data-ttu-id="72834-149">Ett tomt frågefönster öppnas som är anslutna tooyour databas.</span><span class="sxs-lookup"><span data-stu-id="72834-149">A blank query window opens that is connected tooyour database.</span></span>

3. <span data-ttu-id="72834-150">Ändra hello IP-adress tooyour offentliga IP-adressen i hello frågefönstret och kör följande fråga hello:</span><span class="sxs-lookup"><span data-stu-id="72834-150">In hello query window, modify hello IP address tooyour public IP address and then execute hello following query:</span></span>

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. <span data-ttu-id="72834-151">På verktygsfältet hello **kör** toocreate hello brandväggsregel.</span><span class="sxs-lookup"><span data-stu-id="72834-151">On hello toolbar, click **Execute** toocreate hello firewall rule.</span></span>

## <a name="view-how-tooconnect-an-application-tooyour-database-using-a-secure-connection-string"></a><span data-ttu-id="72834-152">Visa hur tooconnect ett program tooyour databasen med en säker anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="72834-152">View how tooconnect an application tooyour database using a secure connection string</span></span>

<span data-ttu-id="72834-153">tooensure en säker och krypterad anslutning mellan ett klientprogram och en SQL-databas, hello anslutningssträngen har toobe som konfigurerats för att:</span><span class="sxs-lookup"><span data-stu-id="72834-153">tooensure a secure, encrypted connection between a client application and SQL Database, hello connection string has toobe configured to:</span></span>

- <span data-ttu-id="72834-154">Begär en krypterad anslutning och</span><span class="sxs-lookup"><span data-stu-id="72834-154">Request an encrypted connection, and</span></span>
- <span data-ttu-id="72834-155">toonot förtroende hello servercertifikat.</span><span class="sxs-lookup"><span data-stu-id="72834-155">toonot trust hello server certificate.</span></span> 

<span data-ttu-id="72834-156">Detta upprättar en anslutning med Transport Layer Security (TLS) och minskar hello risken för man-in-the-middle-attacker.</span><span class="sxs-lookup"><span data-stu-id="72834-156">This establishes a connection using Transport Layer Security (TLS) and reduces hello risk of man-in-the-middle attacks.</span></span> <span data-ttu-id="72834-157">Du kan få korrekt konfigurerade anslutningssträngar för SQL-databas för klient som stöds drivrutiner från hello Azure-portalen som visas för ADO.net i den här skärmbilden.</span><span class="sxs-lookup"><span data-stu-id="72834-157">You can obtain correctly configured connection strings for your SQL Database for supported client drivers from hello Azure portal as shown for ADO.net in this screenshot.</span></span>

1. <span data-ttu-id="72834-158">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="72834-158">Select **SQL databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span>

2. <span data-ttu-id="72834-159">På hello **översikt** för din databas, klickar du på **visa databasanslutningssträngar**.</span><span class="sxs-lookup"><span data-stu-id="72834-159">On hello **Overview** page for your database, click **Show database connection strings**.</span></span>

3. <span data-ttu-id="72834-160">Granska hello fullständig **ADO.NET** anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="72834-160">Review hello complete **ADO.NET** connection string.</span></span>

    ![ADO.NET-anslutningssträng](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a><span data-ttu-id="72834-162">Skapa databasanvändare</span><span class="sxs-lookup"><span data-stu-id="72834-162">Creating database users</span></span>

<span data-ttu-id="72834-163">Innan du skapar användare, måste du först välja från en av två typer av autentisering som stöds av Azure SQL Database:</span><span class="sxs-lookup"><span data-stu-id="72834-163">Before creating any users, you must first choose from one of two authentication types supported by Azure SQL Database:</span></span> 

<span data-ttu-id="72834-164">**SQL-autentisering**, som använder användarnamn och lösenord för inloggning och användare som är giltig endast i hello kontexten för en viss databas i en logisk server.</span><span class="sxs-lookup"><span data-stu-id="72834-164">**SQL Authentication**, which uses username and password for logins and users that are valid only in hello context of a specific database within a logical server.</span></span> 

<span data-ttu-id="72834-165">**Azure Active Directory-autentisering**, som använder identiteter som hanteras av Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="72834-165">**Azure Active Directory Authentication**, which uses identities managed by Azure Active Directory.</span></span> 

<span data-ttu-id="72834-166">Om du vill toouse [Azure Active Directory](./sql-database-aad-authentication.md) tooauthenticate mot SQL-databasen, en ifylld Azure Active Directory måste finnas innan du kan fortsätta.</span><span class="sxs-lookup"><span data-stu-id="72834-166">If you want toouse [Azure Active Directory](./sql-database-aad-authentication.md) tooauthenticate against SQL Database, a populated Azure Active Directory must exist before you can proceed.</span></span>

<span data-ttu-id="72834-167">Följ dessa steg toocreate en användare som använder SQL-autentisering:</span><span class="sxs-lookup"><span data-stu-id="72834-167">Follow these steps toocreate a user using SQL Authentication:</span></span>

1. <span data-ttu-id="72834-168">Ansluta tooyour databasen, till exempel med [SQL Server Management Studio](./sql-database-connect-query-ssms.md) med dina administratörsautentiseringsuppgifter för servern.</span><span class="sxs-lookup"><span data-stu-id="72834-168">Connect tooyour database, for example using [SQL Server Management Studio](./sql-database-connect-query-ssms.md) using your server admin credentials.</span></span>

2. <span data-ttu-id="72834-169">Högerklicka på hello databasen du vill använda tooadd en ny användare på och klicka på i Object Explorer **ny fråga**.</span><span class="sxs-lookup"><span data-stu-id="72834-169">In Object Explorer, right-click on hello database you want tooadd a new user on and click **New Query**.</span></span> <span data-ttu-id="72834-170">Ett tomt frågefönster öppnas som är anslutna toohello valda databasen.</span><span class="sxs-lookup"><span data-stu-id="72834-170">A blank query window opens that is connected toohello selected database.</span></span>

3. <span data-ttu-id="72834-171">Ange hello följande fråga i frågefönstret hello:</span><span class="sxs-lookup"><span data-stu-id="72834-171">In hello query window, enter hello following query:</span></span>

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. <span data-ttu-id="72834-172">På verktygsfältet hello **kör** toocreate hello användare.</span><span class="sxs-lookup"><span data-stu-id="72834-172">On hello toolbar, click **Execute** toocreate hello user.</span></span>

5. <span data-ttu-id="72834-173">Som standard hello användare kan ansluta toohello databasen men har inte några behörigheter tooread eller skriva data.</span><span class="sxs-lookup"><span data-stu-id="72834-173">By default, hello user can connect toohello database, but has no permissions tooread or write data.</span></span> <span data-ttu-id="72834-174">toogrant dessa behörigheter toohello skapade nyligen användaren, köra hello följande två kommandon i ett nytt frågefönster</span><span class="sxs-lookup"><span data-stu-id="72834-174">toogrant these permissions toohello newly created user, execute hello following two commands in a new query window</span></span>

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

<span data-ttu-id="72834-175">Det är bästa praxis toocreate dessa icke-administratörskonton på hello databasen nivå tooconnect tooyour databasen om du inte behöver tooexecute administratörsåtgärder som skapar nya användare.</span><span class="sxs-lookup"><span data-stu-id="72834-175">It is best practice toocreate these non-administrator accounts at hello database level tooconnect tooyour database unless you need tooexecute administrator tasks like creating new users.</span></span> <span data-ttu-id="72834-176">Granska hello [Azure Active Directory-kursen](./sql-database-aad-authentication-configure.md) om hur tooauthenticate med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="72834-176">Please review hello [Azure Active Directory tutorial](./sql-database-aad-authentication-configure.md) on how tooauthenticate using Azure Active Directory.</span></span>


## <a name="protect-your-data-with-encryption"></a><span data-ttu-id="72834-177">Skydda dina data med kryptering</span><span class="sxs-lookup"><span data-stu-id="72834-177">Protect your data with encryption</span></span>

<span data-ttu-id="72834-178">Azure SQL Database transparent datakryptering (TDE) krypteras automatiskt data i vila, utan några ändringar toohello programmet komma åt hello krypterade databasen.</span><span class="sxs-lookup"><span data-stu-id="72834-178">Azure SQL Database transparent data encryption (TDE) automatically encrypts your data at rest, without requiring any changes toohello application accessing hello encrypted database.</span></span> <span data-ttu-id="72834-179">För nya databaser är TDE aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="72834-179">For newly created databases, TDE is on by default.</span></span> <span data-ttu-id="72834-180">tooenable TDE för din databas eller tooverify TDE är aktiverad, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="72834-180">tooenable TDE for your database or tooverify that TDE is on, follow these steps:</span></span>

1. <span data-ttu-id="72834-181">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="72834-181">Select **SQL databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 

2. <span data-ttu-id="72834-182">Klicka på **Transparent datakryptering** tooopen hello konfigurationssidan för TDE.</span><span class="sxs-lookup"><span data-stu-id="72834-182">Click on **Transparent data encryption** tooopen hello configuration page for TDE.</span></span>

    ![Transparent datakryptering](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. <span data-ttu-id="72834-184">Om det behövs, ange **datakryptering** tooON och på **spara**.</span><span class="sxs-lookup"><span data-stu-id="72834-184">If necessary, set **Data encryption** tooON and click **Save**.</span></span>

<span data-ttu-id="72834-185">hello krypteringsprocessen startar i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="72834-185">hello encryption process starts in hello background.</span></span> <span data-ttu-id="72834-186">Du kan övervaka hello med anslutande tooSQL databasen [SQL Server Management Studio](./sql-database-connect-query-ssms.md) genom att fråga hello encryption_state kolumn i hello `sys.dm_database_encryption_keys` vyn.</span><span class="sxs-lookup"><span data-stu-id="72834-186">You can monitor hello progress by connecting tooSQL Database using [SQL Server Management Studio](./sql-database-connect-query-ssms.md) by querying hello encryption_state column of hello `sys.dm_database_encryption_keys` view.</span></span>

## <a name="enable-sql-database-auditing-if-necessary"></a><span data-ttu-id="72834-187">Aktivera SQL Database auditing, om det behövs</span><span class="sxs-lookup"><span data-stu-id="72834-187">Enable SQL Database auditing, if necessary</span></span>

<span data-ttu-id="72834-188">Azure SQL Database Auditing spårar databashändelser och skriver dem tooan granskningslogg i ditt Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="72834-188">Azure SQL Database Auditing tracks database events and writes them tooan audit log in your Azure Storage account.</span></span> <span data-ttu-id="72834-189">Granskning hjälper dig att upprätthålla regelefterlevnad, Förstå Databasaktivitet och få insyn i avvikelser och fel som kan indikera potentiella säkerhetsöverträdelser.</span><span class="sxs-lookup"><span data-stu-id="72834-189">Auditing can help you maintain regulatory compliance, understand database activity, and gain insight into discrepancies and anomalies that could indicate potential security violations.</span></span> <span data-ttu-id="72834-190">Följ dessa steg toocreate en granskning standardprincip för SQL-databasen:</span><span class="sxs-lookup"><span data-stu-id="72834-190">Follow these steps toocreate a default auditing policy for your SQL database:</span></span>

1. <span data-ttu-id="72834-191">Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan.</span><span class="sxs-lookup"><span data-stu-id="72834-191">Select **SQL databases** from hello left-hand menu, and click your database on hello **SQL databases** page.</span></span> 

2. <span data-ttu-id="72834-192">Välj i hello inställningsbladet **granskning och Hotidentifiering**.</span><span class="sxs-lookup"><span data-stu-id="72834-192">In hello Settings blade, select **Auditing & Threat Detection**.</span></span> <span data-ttu-id="72834-193">Observera att servernivå granskning är diabled och att det finns en **visa inställningarna för** länk som gör att du tooview eller ändra hello server granskningsinställningar från den här kontexten.</span><span class="sxs-lookup"><span data-stu-id="72834-193">Notice that sever-level auditing is diabled and that there is a **View server settings** link that allows you tooview or modify hello server auditing settings from this context.</span></span>

    ![Granskning bladet](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. <span data-ttu-id="72834-195">Om du föredrar tooenable en Audit typ (eller plats?) skiljer sig från hello som har angetts på hello servernivå aktivera **ON** granskning, och välj hello **Blob** Granskningstypen.</span><span class="sxs-lookup"><span data-stu-id="72834-195">If you prefer tooenable an Audit type (or location?) different from hello one specified at hello server level, turn **ON** Auditing, and choose hello **Blob** Auditing Type.</span></span> <span data-ttu-id="72834-196">Om servern blobbgranskning är aktiverat, kommer att finnas hello konfigurerad databasgranskningen bredvid hello server Blob audit.</span><span class="sxs-lookup"><span data-stu-id="72834-196">If server Blob auditing is enabled, hello database configured audit will exist side by side with hello server Blob audit.</span></span>

    ![Aktivera granskning](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. <span data-ttu-id="72834-198">Välj **lagringsinformation** tooopen hello granska loggarna lagring-bladet.</span><span class="sxs-lookup"><span data-stu-id="72834-198">Select **Storage Details** tooopen hello Audit Logs Storage Blade.</span></span> <span data-ttu-id="72834-199">Välj hello Azure storage-konto där loggar sparas och hello kvarhållningsperiod vilka hello gamla loggarna tas bort, klicka sedan på **OK** längst ned hello.</span><span class="sxs-lookup"><span data-stu-id="72834-199">Select hello Azure storage account where logs will be saved, and hello retention period, after which hello old logs will be deleted, then click **OK** at hello bottom.</span></span> 

   > [!TIP]
   > <span data-ttu-id="72834-200">Använd hello samma lagringskonto för alla granskad databaser tooget hello mesta av hello granskning rapporterar mallar.</span><span class="sxs-lookup"><span data-stu-id="72834-200">Use hello same storage account for all audited databases tooget hello most out of hello auditing reports templates.</span></span>
   > 

5. <span data-ttu-id="72834-201">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="72834-201">Click **Save**.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="72834-202">Om du vill ha toocustomize hello granskade händelser kan du göra detta via PowerShell eller REST API - finns hello [Automation (PowerShell / REST-API)](sql-database-auditing.md#subheading-7) mer information.</span><span class="sxs-lookup"><span data-stu-id="72834-202">If you want toocustomize hello audited events, you can do this via PowerShell or REST API - see hello [Automation (PowerShell / REST API)](sql-database-auditing.md#subheading-7) section for more details.</span></span>
>

## <a name="enable-sql-database-threat-detection"></a><span data-ttu-id="72834-203">Aktivera hotidentifiering för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="72834-203">Enable SQL Database threat detection</span></span>

<span data-ttu-id="72834-204">Hotidentifiering innehåller ett nytt lager av säkerhet som gör att kunder toodetect och svarar toopotential hot som de sker genom att tillhandahålla säkerhetsaviseringar på avvikande aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="72834-204">Threat Detection provides a new layer of security, which enables customers toodetect and respond toopotential threats as they occur by providing security alerts on anomalous activities.</span></span> <span data-ttu-id="72834-205">Användare kan utforska hello misstänkta händelser med hjälp av SQL Database Auditing toodetermine om de kommer från en försök tooaccess, överträdelse eller utnyttja data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="72834-205">Users can explore hello suspicious events using SQL Database Auditing toodetermine if they result from an attempt tooaccess, breach or exploit data in hello database.</span></span> <span data-ttu-id="72834-206">Hotidentifiering gör det enkelt tooaddress potentiella hot toohello databas utan hello måste toobe en säkerhetsexpert på eller hantera avancerade säkerhetsövervakning system.</span><span class="sxs-lookup"><span data-stu-id="72834-206">Threat Detection makes it simple tooaddress potential threats toohello database without hello need toobe a security expert or manage advanced security monitoring systems.</span></span>
<span data-ttu-id="72834-207">Hotidentifiering identifierar till exempel vissa avvikande databasaktiviteter som indikerar potentiella försök med SQL injection.</span><span class="sxs-lookup"><span data-stu-id="72834-207">For example, Threat Detection detects certain anomalous database activities indicating potential SQL injection attempts.</span></span> <span data-ttu-id="72834-208">SQL injection är en av hello vanliga Web application säkerhetsproblem på hello Internet, används tooattack datadrivna program.</span><span class="sxs-lookup"><span data-stu-id="72834-208">SQL injection is one of hello common Web application security issues on hello Internet, used tooattack data-driven applications.</span></span> <span data-ttu-id="72834-209">Angripare dra nytta av programmet säkerhetsrisker tooinject skadliga SQL-instruktioner i programmet post fält för brott mot eller ändra data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="72834-209">Attackers take advantage of application vulnerabilities tooinject malicious SQL statements into application entry fields, for breaching or modifying data in hello database.</span></span>

1. <span data-ttu-id="72834-210">Navigera toohello configuration bladet för hello SQL-databas som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="72834-210">Navigate toohello configuration blade of hello SQL database you want toomonitor.</span></span> <span data-ttu-id="72834-211">Välj i hello inställningsbladet **granskning och Hotidentifiering**.</span><span class="sxs-lookup"><span data-stu-id="72834-211">In hello Settings blade, select **Auditing & Threat Detection**.</span></span>

    ![Navigeringsfönstret](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. <span data-ttu-id="72834-213">I hello **granskning och Hotidentifiering** configuration bladet Stäng **ON** granskning, som visar hello threat detection inställningar.</span><span class="sxs-lookup"><span data-stu-id="72834-213">In hello **Auditing & Threat Detection** configuration blade turn **ON** auditing, which will display hello threat detection settings.</span></span>

3. <span data-ttu-id="72834-214">Aktivera **på** hotidentifiering.</span><span class="sxs-lookup"><span data-stu-id="72834-214">Turn **ON** threat detection.</span></span>

4. <span data-ttu-id="72834-215">Konfigurera hello lista med e-postmeddelanden som ska ta emot säkerhetsaviseringar vid identifiering av avvikande databasaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="72834-215">Configure hello list of emails that will receive security alerts upon detection of anomalous database activities.</span></span>

5. <span data-ttu-id="72834-216">Klicka på **spara** i hello **Auditing & Threat detection** bladet toosave hello nya eller uppdaterade granskning och hotidentifiering princip.</span><span class="sxs-lookup"><span data-stu-id="72834-216">Click **Save** in hello **Auditing & Threat detection** blade toosave hello new or updated auditing and threat detection policy.</span></span>

    ![Navigeringsfönstret](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    <span data-ttu-id="72834-218">Om avvikande databasaktiviteter identifieras, får du ett e-postmeddelande vid identifiering av avvikande databasaktiviteter.</span><span class="sxs-lookup"><span data-stu-id="72834-218">If anomalous database activities are detected, you will receive an email notification upon detection of anomalous database activities.</span></span> <span data-ttu-id="72834-219">hello e-post ger information om hello misstänkta säkerhetshändelse inklusive hello uppbyggnad hello avvikande aktiviteter, databasnamn, namn och hello händelse servertid.</span><span class="sxs-lookup"><span data-stu-id="72834-219">hello email will provide information on hello suspicious security event including hello nature of hello anomalous activities, database name, server name and hello event time.</span></span> <span data-ttu-id="72834-220">Dessutom den ger information om möjliga orsaker och rekommenderade åtgärder tooinvestigate och minska hello potentiella hot toohello databas.</span><span class="sxs-lookup"><span data-stu-id="72834-220">In addition, it will provide information on possible causes and recommended actions tooinvestigate and mitigate hello potential threat toohello database.</span></span> <span data-ttu-id="72834-221">hello nästa steg gå igenom vilka toodo bör du ta emot sådan ett e-postmeddelande:</span><span class="sxs-lookup"><span data-stu-id="72834-221">hello next steps walk you through what toodo should you receive such an email:</span></span>

    ![Threat detection e-post](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. <span data-ttu-id="72834-223">I hello e-post, klickar du på hello **Azure SQL-granskning logg** länk som startar hello Azure-portalen och visa hello relevanta granskning poster runt hello tidpunkt hello misstänkta händelsen.</span><span class="sxs-lookup"><span data-stu-id="72834-223">In hello email, click on hello **Azure SQL Auditing Log** link, which will launch hello Azure portal and show hello relevant auditing records around hello time of hello suspicious event.</span></span>

    ![Granskningsposter](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. <span data-ttu-id="72834-225">Klicka på hello audit poster tooview mer information om hello misstänkt databasaktiviteter, till exempel SQL-instruktionen, fel orsak och klient-IP.</span><span class="sxs-lookup"><span data-stu-id="72834-225">Click on hello audit records tooview more details on hello suspicious database activities such as SQL statement, failure reason and client IP.</span></span>

    ![Registrera information](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. <span data-ttu-id="72834-227">I hello granskning poster bladet, klickar du på **öppna i Excel** tooopen förkonfigurerad excel mallen tooimport och kör djupare analys av hello granskningsloggen runt hello tidpunkt hello misstänkta händelsen.</span><span class="sxs-lookup"><span data-stu-id="72834-227">In hello Auditing Records blade, click  **Open in Excel** tooopen a pre-configured excel template tooimport and run deeper analysis of hello audit log around hello time of hello suspicious event.</span></span>

    > [!NOTE]
    > <span data-ttu-id="72834-228">I Excel 2010 eller senare, Power Query och hello **snabb kombinera** inställningen är obligatorisk.</span><span class="sxs-lookup"><span data-stu-id="72834-228">In Excel 2010 or later, Power Query and hello **Fast Combine** setting is required.</span></span>

    ![Öppna poster i Excel](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. <span data-ttu-id="72834-230">tooconfigure hello **snabb kombinera** inställningen - i hello **POWER QUERY** menyfliksområdet fliken **alternativ** toodisplay hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="72834-230">tooconfigure hello **Fast Combine** setting - In hello **POWER QUERY** ribbon tab, select **Options** toodisplay hello Options dialog.</span></span> <span data-ttu-id="72834-231">Välj hello sekretess avsnitt och välj hello andra alternativet - ”Ignorera hello sekretessnivåerna och förbättra prestanda”:</span><span class="sxs-lookup"><span data-stu-id="72834-231">Select hello Privacy section and choose hello second option - 'Ignore hello Privacy Levels and potentially improve performance':</span></span>

    ![Excel snabb kombinera](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. <span data-ttu-id="72834-233">tooload granskningsloggarna för SQL, kontrollera att hello parametrar på fliken Inställningar för hello är korrekt inställda Välj hello ”uppgifter” menyfliksområdet och klickar på ”Uppdatera alla' hello-knappen.</span><span class="sxs-lookup"><span data-stu-id="72834-233">tooload SQL audit logs, ensure that hello parameters in hello settings tab are set correctly and then select hello 'Data' ribbon and click hello 'Refresh All' button.</span></span>

    ![Excel-parametrar](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. <span data-ttu-id="72834-235">hello resultat visas i hello **SQL granskningsloggar** blad som gör att du toorun djupare analys av hello avvikande aktiviteter som har upptäckts och minska hello effekten av hello säkerhetshändelse i ditt program.</span><span class="sxs-lookup"><span data-stu-id="72834-235">hello results appear in hello **SQL Audit Logs** sheet which enables you toorun deeper analysis of hello anomalous activities that were detected, and mitigate hello impact of hello security event in your application.</span></span>


## <a name="next-steps"></a><span data-ttu-id="72834-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72834-236">Next steps</span></span>
<span data-ttu-id="72834-237">Du kan förbättra hello skyddet av databasen mot obehöriga användare eller obehörig åtkomst med bara några få enkla steg.</span><span class="sxs-lookup"><span data-stu-id="72834-237">You can improve hello protection of your database against malicious users or unauthorized access with just a few simple steps.</span></span> <span data-ttu-id="72834-238">I den här kursen lär du dig:</span><span class="sxs-lookup"><span data-stu-id="72834-238">In this tutorial you learn to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="72834-239">Konfigurera brandväggsregler för servern och eller-databas</span><span class="sxs-lookup"><span data-stu-id="72834-239">Set up firewall rules for your sever and or database</span></span>
> * <span data-ttu-id="72834-240">Ansluta tooyour databasen med hjälp av en säker anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="72834-240">Connect tooyour database using a secure connection string</span></span>
> * <span data-ttu-id="72834-241">Hantera användarnas åtkomst</span><span class="sxs-lookup"><span data-stu-id="72834-241">Manage user access</span></span>
> * <span data-ttu-id="72834-242">Skydda dina data med kryptering</span><span class="sxs-lookup"><span data-stu-id="72834-242">Protect your data with encryption</span></span>
> * <span data-ttu-id="72834-243">Aktivera granskning av SQL-databas</span><span class="sxs-lookup"><span data-stu-id="72834-243">Enable SQL Database auditing</span></span>
> * <span data-ttu-id="72834-244">Aktivera hotidentifiering för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="72834-244">Enable SQL Database threat detection</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="72834-245">Förbättra prestanda för SQL-databasen</span><span class="sxs-lookup"><span data-stu-id="72834-245">Improve SQL Database performance</span></span>](sql-database-performance-tutorial.md)

