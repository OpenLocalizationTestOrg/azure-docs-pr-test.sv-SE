---
title: "aaaImplement en fördelade Azure SQL Database-lösning | Microsoft Docs"
description: "Lär dig tooconfigure din Azure SQL Database och program för växling vid fel tooa replikerade databasen och testa redundans."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/26/2017
ms.author: carlrab
ms.openlocfilehash: 9295d33c669405108a1a64ef1e7cb77f582773a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-a-geo-distributed-database"></a><span data-ttu-id="9b21e-103">Implementera en geodistribuerad databas</span><span class="sxs-lookup"><span data-stu-id="9b21e-103">Implement a geo-distributed database</span></span>

<span data-ttu-id="9b21e-104">I den här självstudiekursen, konfigurera en Azure SQL database och program för växling vid fel tooa remote region och testa din plan för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9b21e-104">In this tutorial, you configure an Azure SQL database and application for failover tooa remote region, and then test your failover plan.</span></span> <span data-ttu-id="9b21e-105">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="9b21e-105">You learn how to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="9b21e-106">Skapa databasanvändare och ge dem behörigheter</span><span class="sxs-lookup"><span data-stu-id="9b21e-106">Create database users and grant them permissions</span></span>
> * <span data-ttu-id="9b21e-107">Konfigurera en brandväggsregel på databasnivå</span><span class="sxs-lookup"><span data-stu-id="9b21e-107">Set up a database-level firewall rule</span></span>
> * <span data-ttu-id="9b21e-108">Skapa en [redundansväxlingsgrupp geo-replikering](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="9b21e-108">Create a [geo-replication failover group](sql-database-geo-replication-overview.md)</span></span>
> * <span data-ttu-id="9b21e-109">Skapa och kompilera tooquery en Java-program en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="9b21e-109">Create and compile a Java application tooquery an Azure SQL database</span></span>
> * <span data-ttu-id="9b21e-110">Utför en katastrofåterställning återställningsgranskning</span><span class="sxs-lookup"><span data-stu-id="9b21e-110">Perform a disaster recovery drill</span></span>

<span data-ttu-id="9b21e-111">Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="9b21e-111">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="9b21e-112">Krav</span><span class="sxs-lookup"><span data-stu-id="9b21e-112">Prerequisites</span></span>

<span data-ttu-id="9b21e-113">den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:</span><span class="sxs-lookup"><span data-stu-id="9b21e-113">toocomplete this tutorial, make sure hello following prerequisites are completed:</span></span>

- <span data-ttu-id="9b21e-114">Senaste installerade hello [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="9b21e-114">Installed hello latest [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span></span> 
- <span data-ttu-id="9b21e-115">Installera en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="9b21e-115">Installed an Azure SQL database.</span></span> <span data-ttu-id="9b21e-116">Den här självstudiekursen används exempeldatabasen för hello AdventureWorksLT med namnet **mySampleDatabase** från någon av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="9b21e-116">This tutorial uses hello AdventureWorksLT sample database with a name of **mySampleDatabase** from one of these quick starts:</span></span>

   - [<span data-ttu-id="9b21e-117">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="9b21e-117">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="9b21e-118">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="9b21e-118">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="9b21e-119">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b21e-119">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="9b21e-120">Har identifierat en metod tooexecute SQL-skript mot databasen du använder hello följande fråga verktyg:</span><span class="sxs-lookup"><span data-stu-id="9b21e-120">Have identified a method tooexecute SQL scripts against your database, you can use one of hello following query tools:</span></span>
   - <span data-ttu-id="9b21e-121">Hej frågeredigeraren i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9b21e-121">hello query editor in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9b21e-122">Mer information om hur du använder hello frågeredigeraren i hello Azure-portalen finns [Anslut och fråga med frågeredigeraren](sql-database-get-started-portal.md#query-the-sql-database).</span><span class="sxs-lookup"><span data-stu-id="9b21e-122">For more information on using hello query editor in hello Azure portal, see [Connect and query using Query Editor](sql-database-get-started-portal.md#query-the-sql-database).</span></span>
   - <span data-ttu-id="9b21e-123">hello senaste versionen av [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), vilket är en integrerad miljö för att hantera alla SQL-infrastruktur från SQL Server-tooSQL för Microsoft Windows-databas.</span><span class="sxs-lookup"><span data-stu-id="9b21e-123">hello newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), which is an integrated environment for managing any SQL infrastructure, from SQL Server tooSQL Database for Microsoft Windows.</span></span>
   - <span data-ttu-id="9b21e-124">hello senaste versionen av [Visual Studio Code](https://code.visualstudio.com/docs), vilket är en grafiska redigerare för macOS, Linux och Windows som stöder tillägg, inklusive hello [mssql tillägget](https://aka.ms/mssql-marketplace) för frågor till Microsoft SQL Server , Azure SQL Database och SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9b21e-124">hello newest version of [Visual Studio Code](https://code.visualstudio.com/docs), which is a graphical code editor for Linux, macOS, and Windows that supports extensions, including hello [mssql extension](https://aka.ms/mssql-marketplace) for querying Microsoft SQL Server, Azure SQL Database, and SQL Data Warehouse.</span></span> <span data-ttu-id="9b21e-125">Mer information om hur du använder det här verktyget med Azure SQL Database finns [ansluter och frågar med VS kod](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="9b21e-125">For more information on using this tool with Azure SQL Database, see [Connect and query with VS Code](sql-database-connect-query-vscode.md).</span></span> 

## <a name="create-database-users-and-grant-permissions"></a><span data-ttu-id="9b21e-126">Skapa databasanvändare och bevilja behörigheter</span><span class="sxs-lookup"><span data-stu-id="9b21e-126">Create database users and grant permissions</span></span>

<span data-ttu-id="9b21e-127">Anslut tooyour databas och skapa användarkonton med något av följande fråga verktyg hello:</span><span class="sxs-lookup"><span data-stu-id="9b21e-127">Connect tooyour database and create user accounts using one of hello following query tools:</span></span>

- <span data-ttu-id="9b21e-128">Hej frågeredigeraren i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="9b21e-128">hello Query editor in hello Azure portal</span></span>
- <span data-ttu-id="9b21e-129">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="9b21e-129">SQL Server Management Studio</span></span>
- <span data-ttu-id="9b21e-130">Visual Studio-koden</span><span class="sxs-lookup"><span data-stu-id="9b21e-130">Visual Studio Code</span></span>

<span data-ttu-id="9b21e-131">Dessa användarkonton replikera automatiskt tooyour sekundär server (och hållas synkroniserade).</span><span class="sxs-lookup"><span data-stu-id="9b21e-131">These user accounts replicate automatically tooyour secondary server (and be kept in sync).</span></span> <span data-ttu-id="9b21e-132">toouse SQL Server Management Studio eller Visual Studio Code kan du behöva tooconfigure en brandväggsregel om du ansluter från en klient på en IP-adress som du ännu inte har konfigurerat en brandvägg.</span><span class="sxs-lookup"><span data-stu-id="9b21e-132">toouse SQL Server Management Studio or Visual Studio Code, you may need tooconfigure a firewall rule if you are connecting from a client at an IP address for which you have not yet configured a firewall.</span></span> <span data-ttu-id="9b21e-133">Detaljerade anvisningar finns i [skapa en brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="9b21e-133">For detailed steps, see [Create a server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>

- <span data-ttu-id="9b21e-134">I frågefönstret och kör hello följande fråga toocreate två användarkonton i din databas.</span><span class="sxs-lookup"><span data-stu-id="9b21e-134">In a query window, execute hello following query toocreate two user accounts in your database.</span></span> <span data-ttu-id="9b21e-135">Det här skriptet ger **db_owner** behörigheter toohello **app_admin** konto och ger **Välj** och **uppdatering** behörigheter toohello **app_user** konto.</span><span class="sxs-lookup"><span data-stu-id="9b21e-135">This script grants **db_owner** permissions toohello **app_admin** account and grants **SELECT** and **UPDATE** permissions toohello **app_user** account.</span></span> 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user toodb_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission tooSalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product tooapp_user;
   ```

## <a name="create-database-level-firewall"></a><span data-ttu-id="9b21e-136">Skapa databasnivå brandväggen</span><span class="sxs-lookup"><span data-stu-id="9b21e-136">Create database-level firewall</span></span>

<span data-ttu-id="9b21e-137">Skapa en [databasnivå brandväggsregel](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="9b21e-137">Create a [database-level firewall rule](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) for your SQL database.</span></span> <span data-ttu-id="9b21e-138">Den här databasnivå brandväggsregeln replikerar automatiskt toohello sekundär server som du skapar i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="9b21e-138">This database-level firewall rule replicates automatically toohello secondary server that you create in this tutorial.</span></span> <span data-ttu-id="9b21e-139">För enkelhetens skull (i den här självstudiekursen) stegen Använd hello offentliga IP-adressen hello datorn där du utför hello i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="9b21e-139">For simplicity (in this tutorial), use hello public IP address of hello computer on which you are performing hello steps in this tutorial.</span></span> <span data-ttu-id="9b21e-140">toodetermine hello IP-adress som används för hello servernivå brandväggsregel för den aktuella datorn, se [skapar en brandvägg på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="9b21e-140">toodetermine hello IP address used for hello server-level firewall rule for your current computer, see [Create a server-level firewall](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>  

- <span data-ttu-id="9b21e-141">Ersätt hello föregående fråga i din öppna frågefönstret med hello följande fråga, ersätta hello IP-adresser med hello lämplig IP-adresser för din miljö.</span><span class="sxs-lookup"><span data-stu-id="9b21e-141">In your open query window, replace hello previous query with hello following query, replacing hello IP addresses with hello appropriate IP addresses for your environment.</span></span>  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a><span data-ttu-id="9b21e-142">Skapa en aktiv geo-replikering automatiskt failover-grupp</span><span class="sxs-lookup"><span data-stu-id="9b21e-142">Create an active geo-replication auto failover group</span></span> 

<span data-ttu-id="9b21e-143">Med hjälp av Azure PowerShell, skapa en [aktiv geo-replikering automatisk redundansväxlingsgrupp](sql-database-geo-replication-overview.md) mellan din befintliga Azure SQL server och hello ny tom Azure SQL-server i en Azure-region och Lägg sedan till exempel databasen toohello redundans gruppen.</span><span class="sxs-lookup"><span data-stu-id="9b21e-143">Using Azure PowerShell, create an [active geo-replication auto failover group](sql-database-geo-replication-overview.md) between your existing Azure SQL server and hello new empty Azure SQL server in an Azure region, and then add your sample database toohello failover group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9b21e-144">Dessa cmdletar kräver Azure PowerShell 4.0.</span><span class="sxs-lookup"><span data-stu-id="9b21e-144">These cmdlets require Azure PowerShell 4.0.</span></span> [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. <span data-ttu-id="9b21e-145">Fylla i variabler för din PowerShell-skript med hello värden för din befintliga server och exempeldatabasen, och ange ett globalt unikt värde för redundans gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="9b21e-145">Populate variables for your PowerShell scripts using hello values for your existing server and sample database, and provide a globally unique value for failover group name.</span></span>

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. <span data-ttu-id="9b21e-146">Skapa en tom backup-server i din region för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9b21e-146">Create an empty backup server in your failover region.</span></span>

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. <span data-ttu-id="9b21e-147">Skapa en grupp för växling vid fel mellan hello två servrar.</span><span class="sxs-lookup"><span data-stu-id="9b21e-147">Create a failover group between hello two servers.</span></span>

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      –ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      –FailoverGroupName $myfailovergroupname `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. <span data-ttu-id="9b21e-148">Lägg till din databas toohello failover-grupp.</span><span class="sxs-lookup"><span data-stu-id="9b21e-148">Add your database toohello failover group.</span></span>

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## <a name="install-java-software"></a><span data-ttu-id="9b21e-149">Installera Java-program</span><span class="sxs-lookup"><span data-stu-id="9b21e-149">Install Java software</span></span>

<span data-ttu-id="9b21e-150">hello stegen i det här avsnittet förutsätter att du är bekant med att utveckla med Java och nya tooworking med Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="9b21e-150">hello steps in this section assume that you are familiar with developing using Java and are new tooworking with Azure SQL Database.</span></span> 

### <a name="mac-os"></a><span data-ttu-id="9b21e-151">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="9b21e-151">**Mac OS**</span></span>
<span data-ttu-id="9b21e-152">Öppna terminalen och navigera tooa katalog där du planerar att skapa Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="9b21e-152">Open your terminal and navigate tooa directory where you plan on creating your Java project.</span></span> <span data-ttu-id="9b21e-153">Installera **brew** och **Maven** genom att ange hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="9b21e-153">Install **brew** and **Maven** by entering hello following commands:</span></span> 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

<span data-ttu-id="9b21e-154">Detaljerad information om installation och konfiguration Java och Maven miljö gå hello [skapa en app med SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)väljer **Java**väljer **MacOS**, och följ sedan hello detaljerade instruktioner för att konfigurera Java och Maven i steg 1.2 och 1.3.</span><span class="sxs-lookup"><span data-stu-id="9b21e-154">For detailed guidance on installing and configuring Java and Maven environment, go hello [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **MacOS**, and then follow hello detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="9b21e-155">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="9b21e-155">**Linux (Ubuntu)**</span></span>
<span data-ttu-id="9b21e-156">Öppna terminalen och navigera tooa katalog där du planerar att skapa Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="9b21e-156">Open your terminal and navigate tooa directory where you plan on creating your Java project.</span></span> <span data-ttu-id="9b21e-157">Installera **Maven** genom att ange hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="9b21e-157">Install **Maven** by entering hello following commands:</span></span>

```bash
sudo apt-get install maven
```

<span data-ttu-id="9b21e-158">Detaljerad information om installation och konfiguration Java och Maven miljö gå hello [skapa en app med SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)väljer **Java**väljer **Ubuntu**, och följ sedan hello detaljerade instruktioner för att konfigurera Java och Maven i steg 1.2, 1.3 och 1.4.</span><span class="sxs-lookup"><span data-stu-id="9b21e-158">For detailed guidance on installing and configuring Java and Maven environment, go hello [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **Ubuntu**, and then follow hello detailed instructions for configuring Java and Maven in step 1.2, 1.3, and 1.4.</span></span>

### <a name="windows"></a><span data-ttu-id="9b21e-159">**Windows**</span><span class="sxs-lookup"><span data-stu-id="9b21e-159">**Windows**</span></span>
<span data-ttu-id="9b21e-160">Installera [Maven](https://maven.apache.org/download.cgi) med hello officiella installer.</span><span class="sxs-lookup"><span data-stu-id="9b21e-160">Install [Maven](https://maven.apache.org/download.cgi) using hello official installer.</span></span> <span data-ttu-id="9b21e-161">Använd Maven toohelp hantera beroenden, skapa, testa och köra Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="9b21e-161">Use Maven toohelp manage dependencies, build, test, and run your Java project.</span></span> <span data-ttu-id="9b21e-162">Detaljerad information om installation och konfiguration Java och Maven miljö gå hello [skapa en app med SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)väljer **Java**Windows och välj sedan följa hello detaljerade instruktioner för Konfigurera Java och Maven i steg 1.2 och 1.3.</span><span class="sxs-lookup"><span data-stu-id="9b21e-162">For detailed guidance on installing and configuring Java and Maven environment, go hello [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select Windows, and then follow hello detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

## <a name="create-sqldbsample-project"></a><span data-ttu-id="9b21e-163">Skapa SqlDbSample-projekt</span><span class="sxs-lookup"><span data-stu-id="9b21e-163">Create SqlDbSample project</span></span>

1. <span data-ttu-id="9b21e-164">Skapa ett Maven-projekt i hello kommandokonsolen (till exempel Bash).</span><span class="sxs-lookup"><span data-stu-id="9b21e-164">In hello command console (such as Bash), create a Maven project.</span></span> 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. <span data-ttu-id="9b21e-165">Typen **Y** och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="9b21e-165">Type **Y** and click **Enter**.</span></span>
3. <span data-ttu-id="9b21e-166">Ändra kataloger till det nya projektet.</span><span class="sxs-lookup"><span data-stu-id="9b21e-166">Change directories into your newly created project.</span></span>

   ```bash
   cd SqlDbSamples
   ```

4. <span data-ttu-id="9b21e-167">Använd din favorit redigeraren, öppna hello pom.xml filen i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="9b21e-167">Using your favorite editor, open hello pom.xml file in your project folder.</span></span> 

5. <span data-ttu-id="9b21e-168">Lägg till hello Microsoft JDBC-drivrutinen för SQL Server beroende tooyour Maven-projekt genom att öppna valfri textredigerare och kopiera och klistra in hello följande rader i filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="9b21e-168">Add hello Microsoft JDBC Driver for SQL Server dependency tooyour Maven project by opening your favorite text editor and copying and pasting hello following lines into your pom.xml file.</span></span> <span data-ttu-id="9b21e-169">Skriv inte över hello befintliga värden som är förinställd i hello-filen.</span><span class="sxs-lookup"><span data-stu-id="9b21e-169">Do not overwrite hello existing values prepopulated in hello file.</span></span> <span data-ttu-id="9b21e-170">hello JDBC beroende måste klistras in i hello större ”beroenden” avsnittet (”).</span><span class="sxs-lookup"><span data-stu-id="9b21e-170">hello JDBC dependency must be pasted within hello larger “dependencies” section ( ).</span></span>   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. <span data-ttu-id="9b21e-171">Ange hello version av Java toocompile hello projektet mot genom att lägga till hello efter ”” egenskapsavsnittet i hello pom.xml fil efter hello ”beroenden” avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9b21e-171">Specify hello version of Java toocompile hello project against by adding hello following “properties” section into hello pom.xml file after hello "dependencies" section.</span></span> 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. <span data-ttu-id="9b21e-172">Lägg till följande hello ”Skapa” avsnittet i hello pom.xml filen efter hello ”egenskaper” avsnittet toosupport manifestfiler i burkar.</span><span class="sxs-lookup"><span data-stu-id="9b21e-172">Add hello following "build" section into hello pom.xml file after hello "properties" section toosupport manifest files in jars.</span></span>       

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```
8. <span data-ttu-id="9b21e-173">Spara och Stäng hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="9b21e-173">Save and close hello pom.xml file.</span></span>
9. <span data-ttu-id="9b21e-174">Öppna hello App.java filen (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) och Ersätt hello innehållet med hello efter innehållet.</span><span class="sxs-lookup"><span data-stu-id="9b21e-174">Open hello App.java file (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) and replace hello contents with hello following contents.</span></span> <span data-ttu-id="9b21e-175">Ersätt hello gruppnamn för växling vid fel med hello namn på gruppen för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9b21e-175">Replace hello failover group name with hello name for your failover group.</span></span> <span data-ttu-id="9b21e-176">Om du har ändrat hello värden för hello databasnamn, ändra användarnamn eller lösenord, dessa värden samt.</span><span class="sxs-lookup"><span data-stu-id="9b21e-176">If you have changed hello values for hello database name, user, or password, change those values as well.</span></span>

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";
  
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into hello product table with a unique product name that we can use toofind hello product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query hello data that was previously inserted into hello primary database from hello geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query hello high water mark id that is stored in hello table toobe able toomake unique inserts 
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
        
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```
6. <span data-ttu-id="9b21e-177">Spara och Stäng hello App.java fil.</span><span class="sxs-lookup"><span data-stu-id="9b21e-177">Save and close hello App.java file.</span></span>

## <a name="compile-and-run-hello-sqldbsample-project"></a><span data-ttu-id="9b21e-178">Kompilera och köra hello SqlDbSample projekt</span><span class="sxs-lookup"><span data-stu-id="9b21e-178">Compile and run hello SqlDbSample project</span></span>

1. <span data-ttu-id="9b21e-179">Köra toofollowing kommandot i hello Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="9b21e-179">In hello command console, execute toofollowing command.</span></span>

   ```bash
   mvn package
   ```
2. <span data-ttu-id="9b21e-180">När du är klar kör du hello efter kommandot toorun hello programmet (det körs ungefär en timme om du stoppar den manuellt):</span><span class="sxs-lookup"><span data-stu-id="9b21e-180">When finished, execute hello following command toorun hello application (it runs for about 1 hour unless you stop it manually):</span></span>

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a><span data-ttu-id="9b21e-181">Utföra återställningsgranskning för katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="9b21e-181">Perform disaster recovery drill</span></span>

1. <span data-ttu-id="9b21e-182">Anropa manuell växling för failover-grupp.</span><span class="sxs-lookup"><span data-stu-id="9b21e-182">Call manual failover of failover group.</span></span> 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. <span data-ttu-id="9b21e-183">Observera hello programmet resultat under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="9b21e-183">Observe hello application results during failover.</span></span> <span data-ttu-id="9b21e-184">Vissa infogningar misslyckas medan hello DNS-cachen uppdateras.</span><span class="sxs-lookup"><span data-stu-id="9b21e-184">Some inserts fail while hello DNS cache refreshes.</span></span>     

3. <span data-ttu-id="9b21e-185">Ta reda på vilken roll till disaster recovery-serverns prestanda.</span><span class="sxs-lookup"><span data-stu-id="9b21e-185">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. <span data-ttu-id="9b21e-186">Återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="9b21e-186">Failback.</span></span>

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. <span data-ttu-id="9b21e-187">Observera hello programmet resultat vid återställning.</span><span class="sxs-lookup"><span data-stu-id="9b21e-187">Observe hello application results during failback.</span></span> <span data-ttu-id="9b21e-188">Vissa infogningar misslyckas medan hello DNS-cachen uppdateras.</span><span class="sxs-lookup"><span data-stu-id="9b21e-188">Some inserts fail while hello DNS cache refreshes.</span></span>     

6. <span data-ttu-id="9b21e-189">Ta reda på vilken roll till disaster recovery-serverns prestanda.</span><span class="sxs-lookup"><span data-stu-id="9b21e-189">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## <a name="next-steps"></a><span data-ttu-id="9b21e-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9b21e-190">Next steps</span></span> 

<span data-ttu-id="9b21e-191">Mer information finns i [aktiv geo-replikering och redundans grupper](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9b21e-191">For more information, see [Active geo-replication and failover groups](sql-database-geo-replication-overview.md).</span></span>
