---
title: aaaUse Node.js tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet beskrivs hur du toouse Node.js toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 3870130a486c218eafeb9cf792a4275de7fd6551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-nodejs-tooquery-an-azure-sql-database"></a>Använda Node.js tooquery en Azure SQL database

Den här snabbstartsguide visar hur toouse [Node.js](https://nodejs.org/en/) toocreate ett program tooconnect tooan Azure SQL-databas och använda Transact-SQL-instruktioner tooquery data.

## <a name="prerequisites"></a>Krav

toocomplete detta snabb start kursen, kontrollera att du har hello följande:

- En Azure SQL-databas. Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter: 

   - [Skapa DB – Portal](sql-database-get-started-portal.md)
   - [Skapa DB – CLI](sql-database-get-started-cli.md)
   - [Skapa DB – PowerShell](sql-database-get-started-powershell.md)

- En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.
- Du har installerat Node.js och relaterad programvara för ditt operativsystem.
    - **MacOS**: Installera Homebrew och Node.js och installera sedan hello ODBC-drivrutinen och SQLCMD. Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
    - **Ubuntu**: Installera Node.js och installera sedan hello ODBC-drivrutinen och SQLCMD. Se [steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/) .
    - **Windows**: Installera Chocolatey och Node.js och installera sedan hello ODBC-drivrutinen och SQL CMD. Se [Steg 1.2 och 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="sql-server-connection-information"></a>Anslutningsinformation för en SQL-server

Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas. Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan. 
3. På hello **översikt** för databasen, granska hello fullständigt kvalificerade servernamnet som visas i följande bild hello. Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativet. 

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Om du har glömt hello inloggningsinformation för din Azure SQL Database-server, navigera toohello SQL server sidan tooview hello admin Databasservernamnet och, om nödvändigt återställa hello lösenord.

> [!IMPORTANT]
> Du måste ha en brandväggsregel för hello offentliga IP-adressen hello datorn som du utför den här kursen. Om du är på en annan dator eller en annan offentlig IP-adress, skapar du en [servernivå brandväggen regeln med hjälp av hello Azure-portalen](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="create-a-nodejs-project"></a>Skapa ett Node.js-projekt

Öppna en kommandotolk och skapa en mapp med namnet *sqltest*. Navigera toohello mappen du skapade och kör följande kommando hello:

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-tooquery-sql-database"></a>Infoga kod tooquery SQL-databas

1. Skapa en ny fil **sqltest.js** i din utvecklingsmiljö eller valfri textredigerare.

2. Ersätt hello innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection toodatabase
   var config = 
      {
        userName: 'someuser', // update me
        password: 'somepassword', // update me
        server: 'edmacasqlserver.database.windows.net', // update me
        options: 
           {
              database: 'somedb' //update me
              , encrypt: true
           }
      }
   var connection = new Connection(config);

   // Attempt tooconnect and execute queries if connection goes through
   connection.on('connect', function(err) 
      {
        if (err) 
          {
             console.log(err)
          }
       else
          {
              queryDatabase()
          }
      }
    );

   function queryDatabase()
      { console.log('Reading rows from hello Table...');

          // Read all rows from table
        request = new Request(
             "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
                function(err, rowCount, rows) 
                   {
                       console.log(rowCount + ' row(s) returned');
                       process.exit();
                   }
               );
    
        request.on('row', function(columns) {
           columns.forEach(function(column) {
               console.log("%s\t%s", column.metadata.colName, column.value);
            });
                });
        connection.execSql(request);
      }
```

## <a name="run-hello-code"></a>Köra hello kod

1. Kör följande kommandon hello Kommandotolken hello:

   ```js
   node sqltest.js
   ```

2. Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om hello [Microsoft Node.js Driver för SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)
- Lär dig hur för[Anslut och fråga en Azure SQL database med .NET core](sql-database-connect-query-dotnet-core.md) på macOS/Windows/Linux.  
- Lär dig mer om [komma igång med .NET Core för Windows/Linux/macOS hello kommandoraden](/dotnet/core/tutorials/using-with-xplat-cli).
- Lär dig hur för[utforma din första Azure SQL-databas med hjälp av SSMS](sql-database-design-first-database.md) eller [utforma din första Azure SQL-databas med hjälp av .NET](sql-database-design-first-database-csharp.md).
- Lär dig hur för[Anslut och fråga med SSMS](sql-database-connect-query-ssms.md)
- Lär dig hur för[Anslut och fråga med Visual Studio Code](sql-database-connect-query-vscode.md).


