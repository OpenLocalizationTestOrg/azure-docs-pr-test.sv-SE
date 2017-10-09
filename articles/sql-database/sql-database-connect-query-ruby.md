---
title: aaaUse Ruby tooquery Azure SQL Database | Microsoft Docs
description: "Det här avsnittet beskrivs hur du toouse Ruby toocreate ett program som ansluter tooan Azure SQL Database och fråga dem med hjälp av Transact-SQL-uttryck."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: carlrab
ms.openlocfilehash: 0d4b16b8aacb5e376ab80cbe37569130f2fd52b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-ruby-tooquery-an-azure-sql-database"></a>Använd Ruby tooquery en Azure SQL database

Den här snabbstartsguide visar hur toouse [Ruby](https://www.ruby-lang.org) toocreate ett program tooconnect tooan Azure SQL-databas och använda Transact-SQL-instruktioner tooquery data.

## <a name="prerequisites"></a>Krav

toocomplete detta snabb start kursen, kontrollera att du har hello följande krav:

- En Azure SQL-databas. Den här snabbstartsguide använder hello resurser skapas i ett av dessa snabbstarter: 

   - [Skapa DB – Portal](sql-database-get-started-portal.md)
   - [Skapa DB – CLI](sql-database-get-started-cli.md)
   - [Skapa DB – PowerShell](sql-database-get-started-powershell.md)

- En [brandväggsregel på servernivå](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) för hello offentliga IP-adressen för hello datorn du använder för den här snabbstartsguide.
- Du har installerat Ruby och relaterad programvara för ditt operativsystem.
    - **MacOS**: Installera Homebrew, installera rbenv och Ruby-build, installera Ruby och sedan FreeTDS. Se [steg 1.2, 1.3, 1.4 och 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/mac/).
    - **Ubuntu**: Installera förhandskraven för Ruby, installera rbenv och Ruby-build, installera Ruby och sedan FreeTDS. Se [steg 1.2, 1.3, 1.4 och 1.5](https://www.microsoft.com/sql-server/developer-get-started/ruby/ubuntu/).

## <a name="sql-server-connection-information"></a>Anslutningsinformation för en SQL-server

Hämta hello anslutning information som behövs för tooconnect toohello Azure SQL-databas. Du behöver hello fullständigt kvalificerade servernamnet, databasnamnet och inloggningsinformation i hello nästkommande procedurer.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan. 
3. På hello **översikt** för din databas kan du granska hello fullständigt kvalificerade servernamnet. Du kan hovrar över hello server name toobring in hello **klickar du på toocopy** alternativ, enligt följande bild hello:

   ![server-name](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Om du har glömt hello inloggningsinformation för din Azure SQL Database-server, navigera toohello SQL server sidan tooview hello admin Databasservernamnet och, om nödvändigt återställa hello lösenord.

> [!IMPORTANT]
> Du måste ha en brandväggsregel för hello offentliga IP-adressen hello datorn som du utför den här kursen. Om du är på en annan dator eller en annan offentlig IP-adress, skapar du en [servernivå brandväggen regeln med hjälp av hello Azure-portalen](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="insert-code-tooquery-sql-database"></a>Infoga kod tooquery SQL-databas

1. Skapa en ny fil, **sqltest.py**, i valfri textredigerare

2. Ersätt hello innehållet med hello följande kod och lägga till hello lämpliga värden för din server, databas, användare och lösenord.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## <a name="run-hello-code"></a>Köra hello kod

1. Kör följande kommandon hello Kommandotolken hello:

   ```bash
   ruby sqltest.rb
   ```

2. Kontrollera att hello översta 20 rader returneras och stäng sedan hello-fönstret.


## <a name="next-steps"></a>Nästa steg
- [Utforma din första Azure SQL Database](sql-database-design-first-database.md)
- [GitHub-lagringsplatsen för TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Rapportera problem eller ställ frågor om TinyTDS](https://github.com/rails-sqlserver/tiny_tds/issues)
- [Ruby-drivrutiner för SQLServer](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)
