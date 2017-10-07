---
title: aaaImplement flera innehavare SaaS-program med Azure SQL Database | Microsoft Docs
description: Implementera flera innehavare SaaS-program med Azure SQL Database.
services: sql-database
documentationcenter: 
author: AyoOlubeko
manager: jhubbard
editor: monicar
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,scale out apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/08/2017
ms.author: AyoOlubek
ms.openlocfilehash: b87b8f296e2c20a8f674b56375f43fdc92df76d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="implement-a-multi-tenant-saas-application-using-azure-sql-database"></a><span data-ttu-id="745e7-103">Implementera ett SaaS-program för flera innehavare som använder Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="745e7-103">Implement a multi-tenant SaaS application using Azure SQL Database</span></span>

<span data-ttu-id="745e7-104">Ett program med flera innehavare är ett program som finns i en molnmiljö och som ger hello samma uppsättning tjänster toohundreds eller tusentals klienter som inte delar eller se varandras data.</span><span class="sxs-lookup"><span data-stu-id="745e7-104">A multi-tenant application is an application hosted in a cloud environment and that provides hello same set of services toohundreds or thousands of tenants who do not share or see each other’s data.</span></span> <span data-ttu-id="745e7-105">Ett exempel är en SaaS-program som tillhandahåller tjänster tootenants i en miljö med värd i molnet.</span><span class="sxs-lookup"><span data-stu-id="745e7-105">An example is an SaaS application that provides services tootenants in a cloud-hosted environment.</span></span> <span data-ttu-id="745e7-106">Den här modellen isolerar hello data för varje klient och optimerar hello distribution av resurser för kostnad.</span><span class="sxs-lookup"><span data-stu-id="745e7-106">This model isolates hello data for each tenant and optimizes hello distribution of resources for cost.</span></span> 

<span data-ttu-id="745e7-107">Den här kursen visar hur toocreate ett SaaS-program för flera innehavare som använder Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="745e7-107">This tutorial demonstrates how toocreate a multi-tenant SaaS application using Azure SQL Database.</span></span>

<span data-ttu-id="745e7-108">I kursen får du lära dig:</span><span class="sxs-lookup"><span data-stu-id="745e7-108">In this tutorial, you will learn to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="745e7-109">Konfigurera en databas miljö toosupport ett flera innehavare SaaS-program med hjälp av hello databas per klient mönster</span><span class="sxs-lookup"><span data-stu-id="745e7-109">Set up a database environment toosupport a multi-tenant SaaS application, using hello Database-per-tenant pattern</span></span>
> * <span data-ttu-id="745e7-110">Skapa en klient-katalog</span><span class="sxs-lookup"><span data-stu-id="745e7-110">Create a tenant catalog</span></span>
> * <span data-ttu-id="745e7-111">Etablera en klient-databas och registrera den i katalogen för hello-klient</span><span class="sxs-lookup"><span data-stu-id="745e7-111">Provision a tenant database and register it in hello tenant catalog</span></span>
> * <span data-ttu-id="745e7-112">Ställ in ett exempelprogram för Java</span><span class="sxs-lookup"><span data-stu-id="745e7-112">Set up a sample Java application</span></span> 
> * <span data-ttu-id="745e7-113">Åtkomst till klient databaser enkel ett Java-konsolprogram</span><span class="sxs-lookup"><span data-stu-id="745e7-113">Access tenant databases simple a Java console application</span></span>
> * <span data-ttu-id="745e7-114">Ta bort en klient</span><span class="sxs-lookup"><span data-stu-id="745e7-114">Delete a tenant</span></span>

<span data-ttu-id="745e7-115">Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="745e7-115">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="745e7-116">Krav</span><span class="sxs-lookup"><span data-stu-id="745e7-116">Prerequisites</span></span>

<span data-ttu-id="745e7-117">toocomplete den här självstudiekursen, se till att du har:</span><span class="sxs-lookup"><span data-stu-id="745e7-117">toocomplete this tutorial, make sure you have:</span></span>

* <span data-ttu-id="745e7-118">Installerade hello senaste versionen av PowerShell och hello [senaste Azure PowerShell SDK](http://azure.microsoft.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="745e7-118">Installed hello newest version of PowerShell and hello [latest Azure PowerShell SDK](http://azure.microsoft.com/downloads/)</span></span>

* <span data-ttu-id="745e7-119">Installerade hello senaste versionen av [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="745e7-119">Installed hello latest version of [SQL Server Management Studio](http://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span> <span data-ttu-id="745e7-120">Installera SQL Server Management Studio installeras också hello senaste versionen av SQLPackage, ett kommandoradsverktyg som kan använda tooautomate ett antal uppgifter för utveckling av databasen.</span><span class="sxs-lookup"><span data-stu-id="745e7-120">Installing SQL Server Management Studio also installs hello latest version of SQLPackage, a command-line utility that can be used tooautomate a range of database development tasks.</span></span>

* <span data-ttu-id="745e7-121">Installerade hello [Java Runtime Environment (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) och hello [senaste JAVA Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="745e7-121">Installed hello [Java Runtime Environment (JRE) 8](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) and hello [latest JAVA Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) installed on your machine.</span></span> 

* <span data-ttu-id="745e7-122">Installerat [Apache Maven](https://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="745e7-122">Installed [Apache Maven](https://maven.apache.org/download.cgi).</span></span> <span data-ttu-id="745e7-123">Maven används toohelp hantera beroenden, skapa, testa och köra hello exempelprojektet Java</span><span class="sxs-lookup"><span data-stu-id="745e7-123">Maven will be used toohelp manage dependencies, build, test and run hello sample Java project</span></span>

## <a name="set-up-data-environment"></a><span data-ttu-id="745e7-124">Konfigurera en datamiljö</span><span class="sxs-lookup"><span data-stu-id="745e7-124">Set up data environment</span></span>

<span data-ttu-id="745e7-125">Du kommer etablerar en databas per klient.</span><span class="sxs-lookup"><span data-stu-id="745e7-125">You will be provisioning a database per tenant.</span></span> <span data-ttu-id="745e7-126">hello databas per klient modellen innehåller hello största möjliga isolering mellan klienter med lite DevOps kostnaden.</span><span class="sxs-lookup"><span data-stu-id="745e7-126">hello database-per-tenant model provides hello highest degree of isolation between tenants, with little DevOps cost.</span></span> <span data-ttu-id="745e7-127">toooptimize hello kostnader för molnresurser, du kommer också att etablering hello klient databaser till en elastisk pool där du toooptimize hello pris prestanda för en grupp databaser.</span><span class="sxs-lookup"><span data-stu-id="745e7-127">toooptimize hello cost of cloud resources, you will also be provisioning hello tenant databases into an elastic pool which allows you toooptimize hello price performance for a group of databases.</span></span> <span data-ttu-id="745e7-128">toolearn om andra databasen etablering modeller [visas här](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).</span><span class="sxs-lookup"><span data-stu-id="745e7-128">toolearn about other database provisioning models [see here](sql-database-design-patterns-multi-tenancy-saas-applications.md#multi-tenant-data-models).</span></span>

<span data-ttu-id="745e7-129">Följ dessa steg toocreate en SQLServer och en elastisk pool som är värd för alla klient-databaser.</span><span class="sxs-lookup"><span data-stu-id="745e7-129">Follow these steps toocreate a SQL server and an elastic pool that will host all your tenant databases.</span></span> 

1. <span data-ttu-id="745e7-130">Skapa variabler toostore värden som ska användas i hello resten av hello kursen.</span><span class="sxs-lookup"><span data-stu-id="745e7-130">Create variables toostore values that will be used in hello rest of hello tutorial.</span></span> <span data-ttu-id="745e7-131">Kontrollera att toomodify hello IP-adress variabeln tooinclude IP-adress</span><span class="sxs-lookup"><span data-stu-id="745e7-131">Make sure toomodify hello IP address variable tooinclude your IP address</span></span> 
   
   ```PowerShell 
   # Set an admin login and password for your database
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   
   # Create random unique names for logical server and tenants
   $servername = "server-$(Get-Random)"
   $tenant1 = "geolamice"
   $tenant2 = "ranplex"
   
   # Store current client IP address (modify tooinclude your IP address)
   $startIpAddress = 0.0.0.0 
   $endIpAddress = 0.0.0.0
   ```
   
2. <span data-ttu-id="745e7-132">Inloggningen tooAzure och skapa en SQL server och elastisk pool</span><span class="sxs-lookup"><span data-stu-id="745e7-132">Login tooAzure and create a SQL server and elastic pool</span></span> 
   
   ```PowerShell
   # Login tooAzure 
   Login-AzureRmAccount
   
   # Create resource group 
   New-AzureRmResourceGroup -Name "myResourceGroup" -Location "northcentralus"
   
   # Create logical SQL Server with firewall rules 
   New-AzureRmSqlServer -ResourceGroupName "myResourceGroup" `
       -ServerName $servername `
       -Location "northcentralus" `
       -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   
   New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
       -ServerName $servername `
       -FirewallRuleName "singleAddress" -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
   
   # Create elastic pool 
   New-AzureRmSqlElasticPool -ResourceGroupName "myResourceGroup"
       -ServerName $servername `
       -ElasticPoolName "myElasticPool" `
       -Edition "Standard" `
       -Dtu 50 `
       -DatabaseDtuMin 10 `
       -DatabaseDtuMax 20
   ```
   
## <a name="create-tenant-catalog"></a><span data-ttu-id="745e7-133">Skapa klient katalog</span><span class="sxs-lookup"><span data-stu-id="745e7-133">Create tenant catalog</span></span> 

<span data-ttu-id="745e7-134">I ett SaaS-program för flera innehavare är det viktigt tooknow där information för en klient lagras.</span><span class="sxs-lookup"><span data-stu-id="745e7-134">In a multi-tenant SaaS application, it’s important tooknow where information for a tenant is stored.</span></span> <span data-ttu-id="745e7-135">Detta är vanligtvis lagras i en katalog-databas.</span><span class="sxs-lookup"><span data-stu-id="745e7-135">This is commonly stored in a catalog database.</span></span> <span data-ttu-id="745e7-136">hello katalogdatabasen är används toohold en mappning mellan en klient och en databas som den klientorganisationen data lagras.</span><span class="sxs-lookup"><span data-stu-id="745e7-136">hello catalog database is used toohold a mapping between a tenant and a database in which that tenant’s data is stored.</span></span>  <span data-ttu-id="745e7-137">hello grundläggande mönster tillämpas om flera innehavare eller en enskild klient-databasen används.</span><span class="sxs-lookup"><span data-stu-id="745e7-137">hello basic pattern applies whether a multi-tenant or a single-tenant database is used.</span></span>

<span data-ttu-id="745e7-138">Följ dessa steg toocreate en katalog databas för hello SaaS exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="745e7-138">Follow these steps toocreate a catalog database for hello sample SaaS application.</span></span>

```PowerShell
# Create empty database in pool
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName "tenantCatalog" `
    -ElasticPoolName "myElasticPool"

# Create table tootrack mapping between tenants and their databases
$commandText = "
CREATE TABLE Tenants
(
   TenantId         INT IDENTITY PRIMARY KEY,
   TenantName       NVARCHAR(128) NOT NULL,
   TenantDatabase   NVARCHAR(128) NOT NULL
);

CREATE INDEX IX_TenantName ON Tenants (TenantName);"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="provision-database-for-tenant1-and-register-in-tenant-catalog"></a><span data-ttu-id="745e7-139">Etablera databas för 'tenant1' och registrera i katalogen för klient</span><span class="sxs-lookup"><span data-stu-id="745e7-139">Provision database for 'tenant1' and register in tenant catalog</span></span> 
<span data-ttu-id="745e7-140">Använd Powershell tooprovision en databas för en ny klient 'tenant1' och registrera den här innehavaren i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="745e7-140">Use Powershell tooprovision a database for a new tenant 'tenant1' and register this tenant in hello catalog.</span></span> 

```PowerShell
# Create empty database in pool for 'tenant1'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant1 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into hello table 
$commandText = "
CREATE TABLE WhoAmI (TenantName NVARCHAR(128) NOT NULL);
INSERT INTO WhoAmI VALUES ('Tenant $tenant1');"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database $tenant1 `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Register 'tenant1' in hello tenant catalog 
$commandText = "
INSERT INTO Tenants VALUES ('$tenant1', '$tenant1');"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="provision-database-for-tenant2-and-register-in-tenant-catalog"></a><span data-ttu-id="745e7-141">Etablera databas för 'tenant2' och registrera i katalogen för klient</span><span class="sxs-lookup"><span data-stu-id="745e7-141">Provision database for 'tenant2' and register in tenant catalog</span></span>
<span data-ttu-id="745e7-142">Använd Powershell tooprovision en databas för en ny klient 'tenant2' och registrera den här innehavaren i hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="745e7-142">Use Powershell tooprovision a database for a new tenant 'tenant2' and register this tenant in hello catalog.</span></span> 

```PowerShell
# Create empty database in pool for 'tenant2'
New-AzureRmSqlDatabase  -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant2 `
    -ElasticPoolName "myElasticPool"

# Create table WhoAmI and insert tenant name into hello table 
$commandText = "
CREATE TABLE WhoAmI (TenantName NVARCHAR(128) NOT NULL);
INSERT INTO WhoAmI VALUES ('Tenant $tenant2');"

Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database $tenant2 `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Register tenant 'tenant2' in hello tenant catalog 
$commandText = "
INSERT INTO Tenants VALUES ('$tenant2', '$tenant2');"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection
```

## <a name="set-up-sample-java-application"></a><span data-ttu-id="745e7-143">Ställa in Java exempelprogrammet</span><span class="sxs-lookup"><span data-stu-id="745e7-143">Set up sample Java application</span></span> 

1. <span data-ttu-id="745e7-144">Skapa ett maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="745e7-144">Create a maven project.</span></span> <span data-ttu-id="745e7-145">Skriver hello följande i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="745e7-145">Type hello following in a command prompt window:</span></span>
   
   ```
   mvn archetype:generate -DgroupId=com.microsoft.sqlserver -DartifactId=mssql-jdbc -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```
   
2. <span data-ttu-id="745e7-146">Lägg till den här beroende, språknivå och skapa alternativet toosupport manifestfiler i burkar toohello pom.xml filen:</span><span class="sxs-lookup"><span data-stu-id="745e7-146">Add this dependency, language level, and build option toosupport manifest files in jars toohello pom.xml file:</span></span>
   
   ```XML
   <dependency>
         <groupId>com.microsoft.sqlserver</groupId>
         <artifactId>mssql-jdbc</artifactId>
         <version>6.1.0.jre8</version>
   </dependency>
   
   <properties>
         <maven.compiler.source>1.8</maven.compiler.source>
         <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   
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

3. <span data-ttu-id="745e7-147">Lägg till följande hello till hello App.java fil:</span><span class="sxs-lookup"><span data-stu-id="745e7-147">Add hello following into hello App.java file:</span></span>

   ```java 
   package com.sqldbsamples;
   
   import java.util.Map;
   
   import java.util.HashMap;
   
   import java.io.BufferedReader;
   
   import java.io.InputStreamReader;
   
   import java.sql.Connection;
   
   import java.sql.Statement;
   
   import java.sql.PreparedStatement;
   
   import java.sql.ResultSet;
   
   import java.sql.DriverManager;
   
   public class App {
   
   private static final String SERVER_NAME = "your-server-name";
   
   private static final String CATALOG_DB_NAME = "tenantCatalog";
   
   private static final String USER = "ServerAdmin";
   
   private static final String PASSWORD = "ChangeYourAdminPassword1";
   
   private static final String CATALOG_DB_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", SERVER_NAME, CATALOG_DB_NAME, USER, PASSWORD);
   
   private static final String CMD_LIST = "LIST";

   private static final String CMD_QUERY = "QUERY";

   private static final String CMD_QUIT = "QUIT";
   
   public static void main(String[] args) {
   
   System.out.println("\n############################");
   
   System.out.println("## SAAS DATABASE TUTORIAL ##");
   
   System.out.println("############################\n");
   
   System.out.println("OPTIONS");
   
   System.out.println(" " + CMD_LIST + " - list tenants");
   
   System.out.println(" " + CMD_QUERY + " <NAME> - connect and tenant query tenant <NAME>");
   
   System.out.println(" " + CMD_QUIT + " - quit hello application\n");
   
   try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
   
   while(true) {
   
   String[] input = br.readLine().split("\\s");
   
   if (null != input && input.length > 0) {
   
   if (input[0].equalsIgnoreCase(CMD_LIST)) {
   
   listTenants();
   
   } else if (input[0].toLowerCase().startsWith(CMD_QUERY.toLowerCase()) && input.length == 2) {
   
   queryTenant(input[1].trim());
   
   } else if (input[0].equalsIgnoreCase(CMD_QUIT)) {
   
   break;
   
   } else {
   
   System.out.println(" -> Command not supported");
   
   }
   
   }
   
   System.out.println("");
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   }
   
   private static void listTenants() {
   
   // List all tenants that currently exist in hello system
   
   String sql = "SELECT TenantName FROM Tenants";
   
   try (Connection connection = DriverManager.getConnection(CATALOG_DB_URL);
   
   Statement stmt = connection.createStatement();
   
   ResultSet resultSet = stmt.executeQuery(sql)) {
   
   while (resultSet.next()) {
   
   System.out.println(" -> " + resultSet.getString(1));
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   }
   
   private static void queryTenant(String name) {
   
   // Query hello data that was previously inserted into hello primary database from hello geo replicated database
   
   String url = null;
   
   String sql = "SELECT TenantDatabase FROM Tenants WHERE TenantName = ?";
   
   try (Connection connection = DriverManager.getConnection(CATALOG_DB_URL);
   
   PreparedStatement pstmt = connection.prepareStatement(sql)) {
   
   pstmt.setString(1, name);
   
   try (ResultSet resultSet = pstmt.executeQuery()) {
   
   if (resultSet.next()) {
   
   url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", SERVER_NAME, resultSet.getString(1), USER, PASSWORD);
   
   }
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   if (null != url) {
   
   String tenantSql = "SELECT TenantName FROM WhoAmI";
   
   try (Connection connection = DriverManager.getConnection(url);
   
   Statement stmt = connection.createStatement();

   ResultSet resultSet = stmt.executeQuery(tenantSql)) {
   
   while (resultSet.next()) {
   
   System.out.println(" -> Entry in table WhoAmI in tenant " + name + " is: " + resultSet.getString(1));
   
   }
   
   } catch (Exception e) {
   
   System.out.println(e.getMessage());
   
   e.printStackTrace();
   
   }
   
   } else {
   
   System.out.println(" -> Tenant " + name + " not found");
   
   }
   
   }
   
   }
   ```

4. <span data-ttu-id="745e7-148">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="745e7-148">Save hello file.</span></span>

5. <span data-ttu-id="745e7-149">Gå toocommand konsolen och köra</span><span class="sxs-lookup"><span data-stu-id="745e7-149">Go toocommand console and execute</span></span>

   ```bash
   mvn package
   ```

6. <span data-ttu-id="745e7-150">När du är klar kör du följande toorun hello programmet hello</span><span class="sxs-lookup"><span data-stu-id="745e7-150">When finished, execute hello following toorun hello application</span></span> 
   
   ```
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   ```
   
<span data-ttu-id="745e7-151">hello utdata ser ut så här om den har körts:</span><span class="sxs-lookup"><span data-stu-id="745e7-151">hello output will look like this if it runs successfully:</span></span>

```
############################

## SAAS DATABASE TUTORIAL ##

############################

OPTIONS

LIST - list tenants

QUERY <NAME> - connect and tenant query tenant <NAME>

QUIT - quit hello application

* List hello tenants

* Query tenants you created
```

## <a name="delete-first-tenant"></a><span data-ttu-id="745e7-152">Ta bort första klient</span><span class="sxs-lookup"><span data-stu-id="745e7-152">Delete first tenant</span></span> 
<span data-ttu-id="745e7-153">Använd PowerShell toodelete hello klient databasen och katalogen posten för hello första klient.</span><span class="sxs-lookup"><span data-stu-id="745e7-153">Use PowerShell toodelete hello tenant database and catalog entry for hello first tenant.</span></span>

```PowerShell
# Remove 'tenant1' from catalog 
$commandText = "DELETE FROM Tenants WHERE TenantName = '$tenant1';"
Invoke-SqlCmd `
    -Username $adminlogin `
    -Password $password `
    -ServerInstance ($servername + ".database.windows.net") `
    -Database "tenantCatalog" `
    -ConnectionTimeout 30 `
    -Query $commandText `
    -EncryptConnection

# Delete database 
Remove-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -DatabaseName $tenant1
```

<span data-ttu-id="745e7-154">Försök att ansluta för med 'tenant1' hello Java-program.</span><span class="sxs-lookup"><span data-stu-id="745e7-154">Try connecting too'tenant1' using hello Java application.</span></span> <span data-ttu-id="745e7-155">Du får ett felmeddelande om att hello tenant inte finns.</span><span class="sxs-lookup"><span data-stu-id="745e7-155">You will get an error stating that hello tenant does not exist.</span></span>

## <a name="next-steps"></a><span data-ttu-id="745e7-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="745e7-156">Next steps</span></span> 

<span data-ttu-id="745e7-157">I kursen får du lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="745e7-157">In this tutorial, you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="745e7-158">Konfigurera en databas miljö toosupport ett flera innehavare SaaS-program med hjälp av hello databas per klient mönster</span><span class="sxs-lookup"><span data-stu-id="745e7-158">Set up a database environment toosupport a multi-tenant SaaS application, using hello Database-per-tenant pattern</span></span>
> * <span data-ttu-id="745e7-159">Skapa en klient-katalog</span><span class="sxs-lookup"><span data-stu-id="745e7-159">Create a tenant catalog</span></span>
> * <span data-ttu-id="745e7-160">Etablera en klient-databas och registrera den i katalogen för hello-klient</span><span class="sxs-lookup"><span data-stu-id="745e7-160">Provision a tenant database and register it in hello tenant catalog</span></span>
> * <span data-ttu-id="745e7-161">Ställ in ett exempelprogram för Java</span><span class="sxs-lookup"><span data-stu-id="745e7-161">Set up a sample Java application</span></span> 
> * <span data-ttu-id="745e7-162">Åtkomst till klient databaser enkel ett Java-konsolprogram</span><span class="sxs-lookup"><span data-stu-id="745e7-162">Access tenant databases simple a Java console application</span></span>
> * <span data-ttu-id="745e7-163">Ta bort en klient</span><span class="sxs-lookup"><span data-stu-id="745e7-163">Delete a tenant</span></span>

* <span data-ttu-id="745e7-164">PowerShell-exempel för vanliga uppgifter finns [SQL Database PowerShell-exempel](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)</span><span class="sxs-lookup"><span data-stu-id="745e7-164">PowerShell samples for common tasks, see [SQL Database PowerShell samples](https://docs.microsoft.com/azure/sql-database/sql-database-powershell-samples)</span></span>

* <span data-ttu-id="745e7-165">Designmönster för flera innehavare SaaS-program finns [designmönster](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)</span><span class="sxs-lookup"><span data-stu-id="745e7-165">Design patterns for multi-tenant SaaS applications see [Design patterns](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)</span></span>

* <span data-ttu-id="745e7-166">Java-exempel för vanliga uppgifter för Azure, se [Java-Utvecklingscenter](https://azure.microsoft.com/develop/java/)</span><span class="sxs-lookup"><span data-stu-id="745e7-166">Java samples for common Azure tasks, see [Java Developer Center](https://azure.microsoft.com/develop/java/)</span></span>



