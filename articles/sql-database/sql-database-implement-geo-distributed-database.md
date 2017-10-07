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
# <a name="implement-a-geo-distributed-database"></a>Implementera en geodistribuerad databas

I den här självstudiekursen, konfigurera en Azure SQL database och program för växling vid fel tooa remote region och testa din plan för växling vid fel. Lär dig att: 

> [!div class="checklist"]
> * Skapa databasanvändare och ge dem behörigheter
> * Konfigurera en brandväggsregel på databasnivå
> * Skapa en [redundansväxlingsgrupp geo-replikering](sql-database-geo-replication-overview.md)
> * Skapa och kompilera tooquery en Java-program en Azure SQL database
> * Utför en katastrofåterställning återställningsgranskning

Om du inte har en Azure-prenumeration [skapa ett kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.


## <a name="prerequisites"></a>Krav

den här självstudiekursen, se till att hello följande krav är slutförda toocomplete:

- Senaste installerade hello [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs). 
- Installera en Azure SQL database. Den här självstudiekursen används exempeldatabasen för hello AdventureWorksLT med namnet **mySampleDatabase** från någon av dessa snabbstarter:

   - [Skapa DB – Portal](sql-database-get-started-portal.md)
   - [Skapa DB – CLI](sql-database-get-started-cli.md)
   - [Skapa DB – PowerShell](sql-database-get-started-powershell.md)

- Har identifierat en metod tooexecute SQL-skript mot databasen du använder hello följande fråga verktyg:
   - Hej frågeredigeraren i hello [Azure-portalen](https://portal.azure.com). Mer information om hur du använder hello frågeredigeraren i hello Azure-portalen finns [Anslut och fråga med frågeredigeraren](sql-database-get-started-portal.md#query-the-sql-database).
   - hello senaste versionen av [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), vilket är en integrerad miljö för att hantera alla SQL-infrastruktur från SQL Server-tooSQL för Microsoft Windows-databas.
   - hello senaste versionen av [Visual Studio Code](https://code.visualstudio.com/docs), vilket är en grafiska redigerare för macOS, Linux och Windows som stöder tillägg, inklusive hello [mssql tillägget](https://aka.ms/mssql-marketplace) för frågor till Microsoft SQL Server , Azure SQL Database och SQL Data Warehouse. Mer information om hur du använder det här verktyget med Azure SQL Database finns [ansluter och frågar med VS kod](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Skapa databasanvändare och bevilja behörigheter

Anslut tooyour databas och skapa användarkonton med något av följande fråga verktyg hello:

- Hej frågeredigeraren i hello Azure-portalen
- SQL Server Management Studio
- Visual Studio-koden

Dessa användarkonton replikera automatiskt tooyour sekundär server (och hållas synkroniserade). toouse SQL Server Management Studio eller Visual Studio Code kan du behöva tooconfigure en brandväggsregel om du ansluter från en klient på en IP-adress som du ännu inte har konfigurerat en brandvägg. Detaljerade anvisningar finns i [skapa en brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- I frågefönstret och kör hello följande fråga toocreate två användarkonton i din databas. Det här skriptet ger **db_owner** behörigheter toohello **app_admin** konto och ger **Välj** och **uppdatering** behörigheter toohello **app_user** konto. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user toodb_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission tooSalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product tooapp_user;
   ```

## <a name="create-database-level-firewall"></a>Skapa databasnivå brandväggen

Skapa en [databasnivå brandväggsregel](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) för SQL-databas. Den här databasnivå brandväggsregeln replikerar automatiskt toohello sekundär server som du skapar i den här kursen. För enkelhetens skull (i den här självstudiekursen) stegen Använd hello offentliga IP-adressen hello datorn där du utför hello i den här kursen. toodetermine hello IP-adress som används för hello servernivå brandväggsregel för den aktuella datorn, se [skapar en brandvägg på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).  

- Ersätt hello föregående fråga i din öppna frågefönstret med hello följande fråga, ersätta hello IP-adresser med hello lämplig IP-adresser för din miljö.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Skapa en aktiv geo-replikering automatiskt failover-grupp 

Med hjälp av Azure PowerShell, skapa en [aktiv geo-replikering automatisk redundansväxlingsgrupp](sql-database-geo-replication-overview.md) mellan din befintliga Azure SQL server och hello ny tom Azure SQL-server i en Azure-region och Lägg sedan till exempel databasen toohello redundans gruppen.

> [!IMPORTANT]
> Dessa cmdletar kräver Azure PowerShell 4.0. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Fylla i variabler för din PowerShell-skript med hello värden för din befintliga server och exempeldatabasen, och ange ett globalt unikt värde för redundans gruppnamn.

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

2. Skapa en tom backup-server i din region för växling vid fel.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. Skapa en grupp för växling vid fel mellan hello två servrar.

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

4. Lägg till din databas toohello failover-grupp.

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

## <a name="install-java-software"></a>Installera Java-program

hello stegen i det här avsnittet förutsätter att du är bekant med att utveckla med Java och nya tooworking med Azure SQL Database. 

### <a name="mac-os"></a>**Mac OS**
Öppna terminalen och navigera tooa katalog där du planerar att skapa Java-projekt. Installera **brew** och **Maven** genom att ange hello följande kommandon: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

Detaljerad information om installation och konfiguration Java och Maven miljö gå hello [skapa en app med SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)väljer **Java**väljer **MacOS**, och följ sedan hello detaljerade instruktioner för att konfigurera Java och Maven i steg 1.2 och 1.3.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Öppna terminalen och navigera tooa katalog där du planerar att skapa Java-projekt. Installera **Maven** genom att ange hello följande kommandon:

```bash
sudo apt-get install maven
```

Detaljerad information om installation och konfiguration Java och Maven miljö gå hello [skapa en app med SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)väljer **Java**väljer **Ubuntu**, och följ sedan hello detaljerade instruktioner för att konfigurera Java och Maven i steg 1.2, 1.3 och 1.4.

### <a name="windows"></a>**Windows**
Installera [Maven](https://maven.apache.org/download.cgi) med hello officiella installer. Använd Maven toohelp hantera beroenden, skapa, testa och köra Java-projekt. Detaljerad information om installation och konfiguration Java och Maven miljö gå hello [skapa en app med SQL Server](https://www.microsoft.com/sql-server/developer-get-started/)väljer **Java**Windows och välj sedan följa hello detaljerade instruktioner för Konfigurera Java och Maven i steg 1.2 och 1.3.

## <a name="create-sqldbsample-project"></a>Skapa SqlDbSample-projekt

1. Skapa ett Maven-projekt i hello kommandokonsolen (till exempel Bash). 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Typen **Y** och på **RETUR**.
3. Ändra kataloger till det nya projektet.

   ```bash
   cd SqlDbSamples
   ```

4. Använd din favorit redigeraren, öppna hello pom.xml filen i projektmappen. 

5. Lägg till hello Microsoft JDBC-drivrutinen för SQL Server beroende tooyour Maven-projekt genom att öppna valfri textredigerare och kopiera och klistra in hello följande rader i filen pom.xml. Skriv inte över hello befintliga värden som är förinställd i hello-filen. hello JDBC beroende måste klistras in i hello större ”beroenden” avsnittet (”).   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Ange hello version av Java toocompile hello projektet mot genom att lägga till hello efter ”” egenskapsavsnittet i hello pom.xml fil efter hello ”beroenden” avsnitt. 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Lägg till följande hello ”Skapa” avsnittet i hello pom.xml filen efter hello ”egenskaper” avsnittet toosupport manifestfiler i burkar.       

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
8. Spara och Stäng hello pom.xml fil.
9. Öppna hello App.java filen (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) och Ersätt hello innehållet med hello efter innehållet. Ersätt hello gruppnamn för växling vid fel med hello namn på gruppen för växling vid fel. Om du har ändrat hello värden för hello databasnamn, ändra användarnamn eller lösenord, dessa värden samt.

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
6. Spara och Stäng hello App.java fil.

## <a name="compile-and-run-hello-sqldbsample-project"></a>Kompilera och köra hello SqlDbSample projekt

1. Köra toofollowing kommandot i hello Kommandotolken.

   ```bash
   mvn package
   ```
2. När du är klar kör du hello efter kommandot toorun hello programmet (det körs ungefär en timme om du stoppar den manuellt):

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Utföra återställningsgranskning för katastrofåterställning

1. Anropa manuell växling för failover-grupp. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Observera hello programmet resultat under växling vid fel. Vissa infogningar misslyckas medan hello DNS-cachen uppdateras.     

3. Ta reda på vilken roll till disaster recovery-serverns prestanda.

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. Återställning efter fel.

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. Observera hello programmet resultat vid återställning. Vissa infogningar misslyckas medan hello DNS-cachen uppdateras.     

6. Ta reda på vilken roll till disaster recovery-serverns prestanda.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```
## <a name="next-steps"></a>Nästa steg 

Mer information finns i [aktiv geo-replikering och redundans grupper](sql-database-geo-replication-overview.md).
