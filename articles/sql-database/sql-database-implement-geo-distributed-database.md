---
title: "Implementera en lösning för fördelade Azure SQL Database | Microsoft Docs"
description: "Lär dig att konfigurera din Azure SQL Database och program för växling vid fel till en replikerad databas och testa redundans."
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
ms.openlocfilehash: 9f53f318e20dac9248906bdbe898ba4dacb286ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="implement-a-geo-distributed-database"></a><span data-ttu-id="c6e8d-103">Implementera en geodistribuerad databas</span><span class="sxs-lookup"><span data-stu-id="c6e8d-103">Implement a geo-distributed database</span></span>

<span data-ttu-id="c6e8d-104">I den här självstudiekursen, konfigurera en Azure SQL database och program för växling vid fel till en fjärransluten region och testa din plan för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-104">In this tutorial, you configure an Azure SQL database and application for failover to a remote region, and then test your failover plan.</span></span> <span data-ttu-id="c6e8d-105">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="c6e8d-105">You learn how to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="c6e8d-106">Skapa databasanvändare och ge dem behörigheter</span><span class="sxs-lookup"><span data-stu-id="c6e8d-106">Create database users and grant them permissions</span></span>
> * <span data-ttu-id="c6e8d-107">Konfigurera en brandväggsregel på databasnivå</span><span class="sxs-lookup"><span data-stu-id="c6e8d-107">Set up a database-level firewall rule</span></span>
> * <span data-ttu-id="c6e8d-108">Skapa en [redundansväxlingsgrupp geo-replikering](sql-database-geo-replication-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c6e8d-108">Create a [geo-replication failover group](sql-database-geo-replication-overview.md)</span></span>
> * <span data-ttu-id="c6e8d-109">Skapa och kompilera ett Java-program att fråga en Azure SQL database</span><span class="sxs-lookup"><span data-stu-id="c6e8d-109">Create and compile a Java application to query an Azure SQL database</span></span>
> * <span data-ttu-id="c6e8d-110">Utför en katastrofåterställning återställningsgranskning</span><span class="sxs-lookup"><span data-stu-id="c6e8d-110">Perform a disaster recovery drill</span></span>

<span data-ttu-id="c6e8d-111">Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-111">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="c6e8d-112">Krav</span><span class="sxs-lookup"><span data-stu-id="c6e8d-112">Prerequisites</span></span>

<span data-ttu-id="c6e8d-113">Följande krav måste uppfyllas för att kunna köra den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="c6e8d-113">To complete this tutorial, make sure the following prerequisites are completed:</span></span>

- <span data-ttu-id="c6e8d-114">Senast installerad [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-114">Installed the latest [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs).</span></span> 
- <span data-ttu-id="c6e8d-115">Installera en Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-115">Installed an Azure SQL database.</span></span> <span data-ttu-id="c6e8d-116">Den här självstudiekursen används exempeldatabasen AdventureWorksLT med namnet **mySampleDatabase** från någon av dessa snabbstarter:</span><span class="sxs-lookup"><span data-stu-id="c6e8d-116">This tutorial uses the AdventureWorksLT sample database with a name of **mySampleDatabase** from one of these quick starts:</span></span>

   - [<span data-ttu-id="c6e8d-117">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="c6e8d-117">Create DB - Portal</span></span>](sql-database-get-started-portal.md)
   - [<span data-ttu-id="c6e8d-118">Skapa DB – CLI</span><span class="sxs-lookup"><span data-stu-id="c6e8d-118">Create DB - CLI</span></span>](sql-database-get-started-cli.md)
   - [<span data-ttu-id="c6e8d-119">Skapa DB – PowerShell</span><span class="sxs-lookup"><span data-stu-id="c6e8d-119">Create DB - PowerShell</span></span>](sql-database-get-started-powershell.md)

- <span data-ttu-id="c6e8d-120">Har identifierat en metod för att köra SQL-skript mot databasen, du kan använda något av följande verktyg i frågan:</span><span class="sxs-lookup"><span data-stu-id="c6e8d-120">Have identified a method to execute SQL scripts against your database, you can use one of the following query tools:</span></span>
   - <span data-ttu-id="c6e8d-121">Frågeredigeraren i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-121">The query editor in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="c6e8d-122">Mer information om hur du använder frågeredigeraren i Azure portal finns [Anslut och fråga med frågeredigeraren](sql-database-get-started-portal.md#query-the-sql-database).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-122">For more information on using the query editor in the Azure portal, see [Connect and query using Query Editor](sql-database-get-started-portal.md#query-the-sql-database).</span></span>
   - <span data-ttu-id="c6e8d-123">Den senaste versionen av [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), vilket är en integrerad miljö för att hantera alla SQL-infrastruktur från SQL Server till SQL-databas för Microsoft Windows.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-123">The newest version of [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), which is an integrated environment for managing any SQL infrastructure, from SQL Server to SQL Database for Microsoft Windows.</span></span>
   - <span data-ttu-id="c6e8d-124">Den senaste versionen av [Visual Studio Code](https://code.visualstudio.com/docs), vilket är en grafiska redigerare för macOS, Linux och Windows som stöder tillägg, inklusive den [mssql tillägget](https://aka.ms/mssql-marketplace) för frågor till Microsoft SQL Server Azure SQL Database och SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-124">The newest version of [Visual Studio Code](https://code.visualstudio.com/docs), which is a graphical code editor for Linux, macOS, and Windows that supports extensions, including the [mssql extension](https://aka.ms/mssql-marketplace) for querying Microsoft SQL Server, Azure SQL Database, and SQL Data Warehouse.</span></span> <span data-ttu-id="c6e8d-125">Mer information om hur du använder det här verktyget med Azure SQL Database finns [ansluter och frågar med VS kod](sql-database-connect-query-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-125">For more information on using this tool with Azure SQL Database, see [Connect and query with VS Code](sql-database-connect-query-vscode.md).</span></span> 

## <a name="create-database-users-and-grant-permissions"></a><span data-ttu-id="c6e8d-126">Skapa databasanvändare och bevilja behörigheter</span><span class="sxs-lookup"><span data-stu-id="c6e8d-126">Create database users and grant permissions</span></span>

<span data-ttu-id="c6e8d-127">Ansluta till din databas och skapa användarkonton med något av följande verktyg i frågan:</span><span class="sxs-lookup"><span data-stu-id="c6e8d-127">Connect to your database and create user accounts using one of the following query tools:</span></span>

- <span data-ttu-id="c6e8d-128">Frågeredigeraren i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c6e8d-128">The Query editor in the Azure portal</span></span>
- <span data-ttu-id="c6e8d-129">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="c6e8d-129">SQL Server Management Studio</span></span>
- <span data-ttu-id="c6e8d-130">Visual Studio-koden</span><span class="sxs-lookup"><span data-stu-id="c6e8d-130">Visual Studio Code</span></span>

<span data-ttu-id="c6e8d-131">Dessa användarkonton replikeras automatiskt till den sekundära servern (och hållas synkroniserade).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-131">These user accounts replicate automatically to your secondary server (and be kept in sync).</span></span> <span data-ttu-id="c6e8d-132">Om du vill använda SQL Server Management Studio eller Visual Studio Code, kan du behöva konfigurera en brandväggsregel om du ansluter från en klient på en IP-adress som du ännu inte har konfigurerat en brandvägg.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-132">To use SQL Server Management Studio or Visual Studio Code, you may need to configure a firewall rule if you are connecting from a client at an IP address for which you have not yet configured a firewall.</span></span> <span data-ttu-id="c6e8d-133">Detaljerade anvisningar finns i [skapa en brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-133">For detailed steps, see [Create a server-level firewall rule](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>

- <span data-ttu-id="c6e8d-134">I frågefönstret och kör följande fråga för att skapa två användarkonton i din databas.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-134">In a query window, execute the following query to create two user accounts in your database.</span></span> <span data-ttu-id="c6e8d-135">Skriptet ger **db_owner** behörigheter till den **app_admin** konto och ger **Välj** och **uppdatering** behörigheter till **app_user** konto.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-135">This script grants **db_owner** permissions to the **app_admin** account and grants **SELECT** and **UPDATE** permissions to the **app_user** account.</span></span> 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## <a name="create-database-level-firewall"></a><span data-ttu-id="c6e8d-136">Skapa databasnivå brandväggen</span><span class="sxs-lookup"><span data-stu-id="c6e8d-136">Create database-level firewall</span></span>

<span data-ttu-id="c6e8d-137">Skapa en [databasnivå brandväggsregel](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-137">Create a [database-level firewall rule](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) for your SQL database.</span></span> <span data-ttu-id="c6e8d-138">Den här databasnivå brandväggsregeln replikerar automatiskt till den sekundära servern som du skapar i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-138">This database-level firewall rule replicates automatically to the secondary server that you create in this tutorial.</span></span> <span data-ttu-id="c6e8d-139">Använd den offentliga IP-adressen på den dator där du utför stegen i den här självstudiekursen för enkelhet (i den här självstudiekursen).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-139">For simplicity (in this tutorial), use the public IP address of the computer on which you are performing the steps in this tutorial.</span></span> <span data-ttu-id="c6e8d-140">Information om IP-adress som används för servernivå brandväggsregeln för den aktuella datorn finns [skapar en brandvägg på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-140">To determine the IP address used for the server-level firewall rule for your current computer, see [Create a server-level firewall](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).</span></span>  

- <span data-ttu-id="c6e8d-141">Ersätt den föregående frågan i din öppna frågefönstret med följande fråga ersätta IP-adresser med lämplig IP-adresser för din miljö.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-141">In your open query window, replace the previous query with the following query, replacing the IP addresses with the appropriate IP addresses for your environment.</span></span>  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a><span data-ttu-id="c6e8d-142">Skapa en aktiv geo-replikering automatiskt failover-grupp</span><span class="sxs-lookup"><span data-stu-id="c6e8d-142">Create an active geo-replication auto failover group</span></span> 

<span data-ttu-id="c6e8d-143">Med hjälp av Azure PowerShell, skapa en [aktiv geo-replikering automatisk redundansväxlingsgrupp](sql-database-geo-replication-overview.md) mellan den befintliga Azure SQL-servern och den nya tomma Azure SQL-server i en Azure-region och sedan lägga till din exempeldatabas gruppen växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-143">Using Azure PowerShell, create an [active geo-replication auto failover group](sql-database-geo-replication-overview.md) between your existing Azure SQL server and the new empty Azure SQL server in an Azure region, and then add your sample database to the failover group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c6e8d-144">Dessa cmdletar kräver Azure PowerShell 4.0.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-144">These cmdlets require Azure PowerShell 4.0.</span></span> [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. <span data-ttu-id="c6e8d-145">Fylla i variabler för din PowerShell-skript med hjälp av värdena för din befintliga server och exempeldatabasen, och ange ett globalt unikt värde för redundans gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-145">Populate variables for your PowerShell scripts using the values for your existing server and sample database, and provide a globally unique value for failover group name.</span></span>

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

2. <span data-ttu-id="c6e8d-146">Skapa en tom backup-server i din region för växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-146">Create an empty backup server in your failover region.</span></span>

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. <span data-ttu-id="c6e8d-147">Skapa en grupp för växling vid fel mellan de två servrarna.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-147">Create a failover group between the two servers.</span></span>

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

4. <span data-ttu-id="c6e8d-148">Lägg till din databas i gruppen växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-148">Add your database to the failover group.</span></span>

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

## <a name="install-java-software"></a><span data-ttu-id="c6e8d-149">Installera Java-program</span><span class="sxs-lookup"><span data-stu-id="c6e8d-149">Install Java software</span></span>

<span data-ttu-id="c6e8d-150">Stegen i det här avsnittet förutsätter att du är bekant med att utveckla med Java och att du är nybörjare när det gäller att arbeta med Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-150">The steps in this section assume that you are familiar with developing using Java and are new to working with Azure SQL Database.</span></span> 

### <a name="mac-os"></a><span data-ttu-id="c6e8d-151">**Mac OS**</span><span class="sxs-lookup"><span data-stu-id="c6e8d-151">**Mac OS**</span></span>
<span data-ttu-id="c6e8d-152">Öppna terminalen och navigera till den katalog där du vill skapa Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-152">Open your terminal and navigate to a directory where you plan on creating your Java project.</span></span> <span data-ttu-id="c6e8d-153">Installera **brew** och **Maven** genom att ange följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c6e8d-153">Install **brew** and **Maven** by entering the following commands:</span></span> 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

<span data-ttu-id="c6e8d-154">Detaljerad information om installation och konfiguration av Java- och Maven-miljö finns i [skapa en app med SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)väljer **Java**väljer **MacOS**, och följ sedan de detaljerade anvisningar för att konfigurera Java och Maven i steg 1.2 och 1.3.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-154">For detailed guidance on installing and configuring Java and Maven environment, go the [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **MacOS**, and then follow the detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="c6e8d-155">**Linux (Ubuntu)**</span><span class="sxs-lookup"><span data-stu-id="c6e8d-155">**Linux (Ubuntu)**</span></span>
<span data-ttu-id="c6e8d-156">Öppna terminalen och navigera till den katalog där du vill skapa Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-156">Open your terminal and navigate to a directory where you plan on creating your Java project.</span></span> <span data-ttu-id="c6e8d-157">Installera **Maven** genom att ange följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c6e8d-157">Install **Maven** by entering the following commands:</span></span>

```bash
sudo apt-get install maven
```

<span data-ttu-id="c6e8d-158">Detaljerad information om installation och konfiguration av Java- och Maven-miljö finns i [skapa en app med SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)väljer **Java**väljer **Ubuntu**, och följ sedan detaljerade anvisningar för att konfigurera Java och Maven i steg 1.2, 1.3 och 1.4.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-158">For detailed guidance on installing and configuring Java and Maven environment, go the [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select **Ubuntu**, and then follow the detailed instructions for configuring Java and Maven in step 1.2, 1.3, and 1.4.</span></span>

### <a name="windows"></a><span data-ttu-id="c6e8d-159">**Windows**</span><span class="sxs-lookup"><span data-stu-id="c6e8d-159">**Windows**</span></span>
<span data-ttu-id="c6e8d-160">Installera [Maven](https://maven.apache.org/download.cgi) med det officiella installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-160">Install [Maven](https://maven.apache.org/download.cgi) using the official installer.</span></span> <span data-ttu-id="c6e8d-161">Använd Maven för att hantera beroenden, skapa, testa och köra Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-161">Use Maven to help manage dependencies, build, test, and run your Java project.</span></span> <span data-ttu-id="c6e8d-162">Detaljerad information om installation och konfiguration av Java- och Maven-miljö finns i [skapa en app med SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)väljer **Java**Windows och välj sedan instruktionerna detaljerat för Konfigurera Java och Maven i steg 1.2 och 1.3.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-162">For detailed guidance on installing and configuring Java and Maven environment, go the [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), select **Java**, select Windows, and then follow the detailed instructions for configuring Java and Maven in step 1.2 and 1.3.</span></span>

## <a name="create-sqldbsample-project"></a><span data-ttu-id="c6e8d-163">Skapa SqlDbSample-projekt</span><span class="sxs-lookup"><span data-stu-id="c6e8d-163">Create SqlDbSample project</span></span>

1. <span data-ttu-id="c6e8d-164">Skapa ett Maven-projekt i kommandokonsolen (till exempel Bash).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-164">In the command console (such as Bash), create a Maven project.</span></span> 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. <span data-ttu-id="c6e8d-165">Typen **Y** och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-165">Type **Y** and click **Enter**.</span></span>
3. <span data-ttu-id="c6e8d-166">Ändra kataloger till det nya projektet.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-166">Change directories into your newly created project.</span></span>

   ```bash
   cd SqlDbSamples
   ```

4. <span data-ttu-id="c6e8d-167">Använd din favorit redigeraren, öppna filen pom.xml i projektmappen.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-167">Using your favorite editor, open the pom.xml file in your project folder.</span></span> 

5. <span data-ttu-id="c6e8d-168">Lägg till Microsoft JDBC-drivrutinen för SQL Server-beroendet till Maven-projekt genom att öppna valfri textredigerare och kopiera och klistra in följande rader i filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-168">Add the Microsoft JDBC Driver for SQL Server dependency to your Maven project by opening your favorite text editor and copying and pasting the following lines into your pom.xml file.</span></span> <span data-ttu-id="c6e8d-169">Skriv inte över de befintliga värden registreringsformuläret i filen.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-169">Do not overwrite the existing values prepopulated in the file.</span></span> <span data-ttu-id="c6e8d-170">JDBC-beroendet måste klistras in i större ”beroenden” avsnittet (-).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-170">The JDBC dependency must be pasted within the larger “dependencies” section ( ).</span></span>   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. <span data-ttu-id="c6e8d-171">Ange version för Java att kompilera projektet mot genom att lägga till egenskapsavsnittet ”” i filen pom.xml efter avsnittet ”beroenden”.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-171">Specify the version of Java to compile the project against by adding the following “properties” section into the pom.xml file after the "dependencies" section.</span></span> 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. <span data-ttu-id="c6e8d-172">Lägg till avsnittet ”Skapa” i filen pom.xml efter avsnittet ”Egenskaper” för att stödja manifestfiler i burkar.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-172">Add the following "build" section into the pom.xml file after the "properties" section to support manifest files in jars.</span></span>       

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
8. <span data-ttu-id="c6e8d-173">Spara och stäng filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-173">Save and close the pom.xml file.</span></span>
9. <span data-ttu-id="c6e8d-174">Öppna filen App.java (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) och Ersätt det med följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-174">Open the App.java file (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) and replace the contents with the following contents.</span></span> <span data-ttu-id="c6e8d-175">Ersätt namnet på failover med namnet för failover-grupp.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-175">Replace the failover group name with the name for your failover group.</span></span> <span data-ttu-id="c6e8d-176">Om du har ändrat värdena för databasens namn, ändra användarnamn eller lösenord, dessa värden samt.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-176">If you have changed the values for the database name, user, or password, change those values as well.</span></span>

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
      // Insert data into the product table with a unique product name that we can use to find the product again later
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
      // Query the data that was previously inserted into the primary database from the geo replicated database
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
      // Query the high water mark id that is stored in the table to be able to make unique inserts 
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
6. <span data-ttu-id="c6e8d-177">Spara och stäng filen App.java.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-177">Save and close the App.java file.</span></span>

## <a name="compile-and-run-the-sqldbsample-project"></a><span data-ttu-id="c6e8d-178">Kompilera och köra projektet SqlDbSample</span><span class="sxs-lookup"><span data-stu-id="c6e8d-178">Compile and run the SqlDbSample project</span></span>

1. <span data-ttu-id="c6e8d-179">Kör följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-179">In the command console, execute to following command.</span></span>

   ```bash
   mvn package
   ```
2. <span data-ttu-id="c6e8d-180">När du är klar kör du följande kommando för att köra programmet (det körs ungefär en timme om du stoppar den manuellt):</span><span class="sxs-lookup"><span data-stu-id="c6e8d-180">When finished, execute the following command to run the application (it runs for about 1 hour unless you stop it manually):</span></span>

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a><span data-ttu-id="c6e8d-181">Utföra återställningsgranskning för katastrofåterställning</span><span class="sxs-lookup"><span data-stu-id="c6e8d-181">Perform disaster recovery drill</span></span>

1. <span data-ttu-id="c6e8d-182">Anropa manuell växling för failover-grupp.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-182">Call manual failover of failover group.</span></span> 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. <span data-ttu-id="c6e8d-183">Se resultaten program under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-183">Observe the application results during failover.</span></span> <span data-ttu-id="c6e8d-184">Vissa infogningar misslyckas medan DNS cachelagra uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-184">Some inserts fail while the DNS cache refreshes.</span></span>     

3. <span data-ttu-id="c6e8d-185">Ta reda på vilken roll till disaster recovery-serverns prestanda.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-185">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. <span data-ttu-id="c6e8d-186">Återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-186">Failback.</span></span>

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. <span data-ttu-id="c6e8d-187">Se resultaten program under återställning efter fel.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-187">Observe the application results during failback.</span></span> <span data-ttu-id="c6e8d-188">Vissa infogningar misslyckas medan DNS cachelagra uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-188">Some inserts fail while the DNS cache refreshes.</span></span>     

6. <span data-ttu-id="c6e8d-189">Ta reda på vilken roll till disaster recovery-serverns prestanda.</span><span class="sxs-lookup"><span data-stu-id="c6e8d-189">Find out which role your disaster recovery server is performing.</span></span>

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## <a name="next-steps"></a><span data-ttu-id="c6e8d-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c6e8d-190">Next steps</span></span> 

<span data-ttu-id="c6e8d-191">Mer information finns i [aktiv geo-replikering och redundans grupper](sql-database-geo-replication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c6e8d-191">For more information, see [Active geo-replication and failover groups](sql-database-geo-replication-overview.md).</span></span>
