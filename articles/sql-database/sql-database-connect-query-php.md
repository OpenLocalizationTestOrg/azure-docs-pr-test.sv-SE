---
title: aaaUse PHP tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet beskrivs hur du toouse PHP toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a 22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: hero-article
ms.date: 08/08/2017
ms.author: carlrab
ms.openlocfilehash: 5fc49dcc42ab07cc1bec554be39bdf08dbd6f75e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-php-tooquery-an-azure-sql-database"></a>Använd PHP tooquery en Azure SQL database

Den här snabbstartsguide visar hur toouse [PHP](http://php.net/manual/en/intro-whatis.php) toocreate ett program tooconnect tooan Azure SQL-databas och använda Transact-SQL-instruktioner tooquery data.

## <a name="prerequisites"></a>Krav

toocomplete detta snabb start kursen, kontrollera att du har hello följande:

- En Azure SQL-databas. Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter: 

   - [Skapa DB – Portal](sql-database-get-started-portal.md)
   - [Skapa DB – CLI](sql-database-get-started-cli.md)
   - [Skapa DB – PowerShell](sql-database-get-started-powershell.md)

- En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.

- Du har installerat PHP och relaterad programvara för ditt operativsystem.

    - **MacOS**: Installera Homebrew och PHP, installera hello ODBC-drivrutinen och SQLCMD och installera sedan hello PHP-drivrutin för SQL Server. Se [steg 1.2, 1.3 och 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).
    - **Ubuntu**: Installera PHP och andra nödvändiga paket och installera hello PHP-drivrutin för SQL Server. Se [steg 1.2 och 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).
    - **Windows**: Installera hello senaste versionen av PHP för IIS Express, hello senaste versionen av Drivers Microsoft för SQL Server i IIS Express, Chocolatey, hello ODBC-drivrutinen och SQLCMD. Se [steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).    

## <a name="sql-server-connection-information"></a>Anslutningsinformation för en SQL-server

Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas. Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan. 
3. På hello **översikt** för databasen, granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello. Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet.  

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Om du glömmer inloggningsinformationen server, navigera toohello SQL server sidan tooview hello admin Databasservernamnet och, om nödvändigt återställa hello lösenord.     
    
## <a name="insert-code-tooquery-sql-database"></a>Infoga kod tooquery SQL-databas

1. Skapa en ny fil, **sqltest.php**, i valfri textredigerare.  

2. Ersätt hello innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes hello connection
   $conn = sqlsrv_connect($serverName, $connectionOptions);
   $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid";
   $getResults= sqlsrv_query($conn, $tsql);
   echo ("Reading data from table" . PHP_EOL);
   if ($getResults == FALSE)
       echo (sqlsrv_errors());
   while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
   }
   sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-hello-code"></a>Köra hello kod

1. Kör följande kommandon hello Kommandotolken hello:

   ```php
   php sqltest.php
   ```

2. Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.

## <a name="next-steps"></a>Nästa steg
- [Utforma din första Azure SQL Database](sql-database-design-first-database.md)
- [Microsoft PHP-drivrutiner för SQL Server](https://github.com/Microsoft/msphpsql/)
- [Rapportera problem eller ställ frågor](https://github.com/Microsoft/msphpsql/issues)
