---
title: "Ansluta tooAzure databas för MySQL med Ruby | Microsoft Docs"
description: "Denna Snabbstart innehåller flera Ruby kodexempel du kan använda tooconnect och fråga efter data från Azure-databas för MySQL."
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.devlang: ruby
ms.topic: hero-article
ms.date: 07/13/2017
ms.openlocfilehash: ff0880dcc24e96f467c9092bc663ce3dc4c2637a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-ruby-tooconnect-and-query-data"></a>Azure-databas för MySQL: Använd Ruby tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure-databas för MySQL med hjälp av en [Ruby](https://www.ruby-lang.org) program och hello [mysql2](https://rubygems.org/gems/mysql2) symbolen från Windows, Ubuntu Linux och Mac-plattformar. Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas. Den här artikeln förutsätter att du är bekant med utveckling med hjälp av Ruby, men som du är ny tooworking med Azure-databas för MySQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa en Azure Database för MySQL med Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Skapa en Azure Database för MySQL-server med Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-ruby"></a>Installera Ruby
Installera Ruby och symbolen hello MySQL2 bibliotek på din egen dator. 

### <a name="windows"></a>Windows
1. Ladda ned och installerar hello 2.3 versionen av [Ruby](http://rubyinstaller.org/downloads/).
2. Starta en ny kommandotolk (cmd) hello Start-menyn.
3. Ändra katalogen till hello Ruby katalog för version 2.3. `cd c:\Ruby23-x64\bin`
4. Testa hello Ruby installationen genom att köra kommandot hello `ruby -v` toosee hello versionen installerad.
5. Testa hello symbolen installationen genom att köra kommandot hello `gem -v` toosee hello versionen installerad.
6. Skapa hello Mysql2-modul för Ruby med symbolen genom att köra kommandot hello `gem install mysql2`.

### <a name="macos"></a>MacOS
1. Installera Ruby med Homebrew genom att köra kommandot hello `brew install ruby`. Flera installationsalternativ finns hello Ruby [dokumentationen](https://www.ruby-lang.org/en/documentation/installation/#homebrew).
2. Testa hello Ruby installationen genom att köra kommandot hello `ruby -v` toosee hello versionen installerad.
3. Testa hello symbolen installationen genom att köra kommandot hello `gem -v` toosee hello versionen installerad.
4. Skapa hello Mysql2-modul för Ruby med symbolen genom att köra kommandot hello `gem install mysql2`.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
1. Installera Ruby genom att köra kommandot hello `sudo apt-get install ruby-full`. Flera installationsalternativ finns hello Ruby [dokumentationen](https://www.ruby-lang.org/en/documentation/installation/).
2. Testa hello Ruby installationen genom att köra kommandot hello `ruby -v` toosee hello versionen installerad.
3. Installera hello senaste uppdateringarna för symbolen genom att köra kommandot hello `sudo gem update --system`.
4. Testa hello symbolen installationen genom att köra kommandot hello `gem -v` toosee hello versionen installerad.
5. Installera hello gcc, märke och andra verktyg genom att köra kommandot hello `sudo apt-get install build-essential`.
6. Installera hello MySQL-klientbibliotek developer genom att köra kommandot hello `sudo apt-get install libmysqlclient-dev`.
7. Skapa hello mysql2-modul för Ruby med symbolen genom att köra kommandot hello `sudo gem install mysql2`.

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har creased, som **myserver4demo**.
3. Klicka på servernamnet för hello **myserver4demo**.
4. Välj hello server **egenskaper** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för MySQL – inloggning för serveradministratör](./media/connect-ruby/1_server-properties-name-login.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.

## <a name="run-ruby-code"></a>Köra Ruby-kod 
1. Klistra in hello Ruby kod från hello avsnitt nedan i textfiler och spara hello filer till en projektmapp med filen tillägget .rb som `C:\rubymysql\createtable.rb` eller `/home/username/rubymysql/createtable.rb`.
2. toorun hello kod, starta Kommandotolken hello eller bash shell. Ändra katalog till din projektmapp, till exempel `cd rubymysql`
3. Skriv hello ruby kommando följt av hello filnamn som `ruby createtable.rb` toorun hello program.
4. På hello Windows-Operativsystemet, om hello ruby programmet inte finns i din path-miljövariabeln måste toouse hello fullständig sökväg toolaunch hello noden program som`"c:\Ruby23-x64\bin\ruby.exe" createtable.rb`

## <a name="connect-and-create-a-table"></a>Ansluta och skapa en tabell
Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen, följt av **INSERT INTO** SQL-instruktioner tooadd rader i hello-tabellen.

hello koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) klassen .new() metoden tooconnect tooAzure databas för MySQL. Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) flera gånger toorun hello släpp CREATE TABLE kommandona, och INSERT INTO. Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello anslutningen innan du avslutar.

Ersätt hello `host`, `database`, `username`, och `password` strängar med egna värden. 
```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Drop previous table of same name if one exists
    client.query('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    client.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    client.query("INSERT INTO inventory VALUES(1, 'banana', 150)")
    client.query("INSERT INTO inventory VALUES(2, 'orange', 154)")
    client.query("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen. 

hello koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) klassen .new() metoden tooconnect tooAzure databas för MySQL. Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello väljer kommandon. Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello anslutningen innan du avslutar.

Ersätt hello `host`, `database`, `username`, och `password` strängar med egna värden. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Read data
    resultSet = client.query('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end
    puts 'Read ' + resultSet.count.to_s + ' row(s).'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.

hello koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) klassen .new() metoden tooconnect tooAzure databas för MySQL. Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello UPDATE kommandon. Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello anslutningen innan du avslutar.

Ersätt hello `host`, `database`, `username`, och `password` strängar med egna värden. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Update data
   client.query('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
   puts 'Updated 1 row of data.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```


## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen. 

hello koden använder en [mysql2::client](http://www.rubydoc.info/gems/mysql2/0.4.8) klassen .new() metoden tooconnect tooAzure databas för MySQL. Sedan anropas metoden [query()](http://www.rubydoc.info/gems/mysql2/0.4.8#Usage) toorun hello DELETE-kommandon. Sedan anropas metoden [close()](http://www.rubydoc.info/gems/mysql2/0.4.8/Mysql2/Client#close-instance_method) tooclose hello anslutningen innan du avslutar.

Ersätt hello `host`, `database`, `username`, och `password` strängar med egna värden. 

```ruby
require 'mysql2'

begin
    # Initialize connection variables.
    host = String('myserver4demo.mysql.database.azure.com')
    database = String('quickstartdb')
    username = String('myadmin@myserver4demo')
    password = String('yourpassword')

    # Initialize connection object.
    client = Mysql2::Client.new(:host => host, :username => username, :database => database, :password => password)
    puts 'Successfully created connection toodatabase.'

    # Delete data
    resultSet = client.query('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row.'

# Error handling
rescue Exception => e
    puts e.message

# Cleanup
ensure
    client.close if client
    puts 'Done.'
end
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./concepts-migrate-import-export.md)
