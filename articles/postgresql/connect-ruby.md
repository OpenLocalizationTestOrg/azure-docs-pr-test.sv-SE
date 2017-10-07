---
title: "aaaConnect tooAzure databas för PostgreSQL med Ruby | Microsoft Docs"
description: "Denna Snabbstart innehåller en Ruby kodexempel som du kan använda tooconnect och fråga efter data från Azure-databas för PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: ruby
ms.topic: quickstart
ms.date: 06/30/2017
ms.openlocfilehash: 7a0c8c92023452b40ca19d76fa659744f3e9a236
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-ruby-tooconnect-and-query-data"></a>Azure-databas för PostgreSQL: Använd Ruby tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure-databas för PostgreSQL med hjälp av en [Ruby](https://www.ruby-lang.org) program. Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas. Den här artikeln förutsätter att du är bekant med utveckling med hjälp av Ruby, men som du är ny tooworking med Azure-databas för PostgreSQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa DB – Portal](quickstart-create-server-database-portal.md)
- [Skapa DB – Azure CLI](quickstart-create-server-database-azure-cli.md)

## <a name="install-ruby"></a>Installera Ruby
Installera Ruby på din egen dator. 

### <a name="windows"></a>Windows
- Hämta och installera hello senaste versionen av [Ruby](http://rubyinstaller.org/downloads/).
- Hello på skärmen hello MSI Installer är klar, kryssrutan hello som säger ”kör ridk installera tooinstall MSYS2 och utveckling toolchain”. Klicka på **Slutför** toolaunch hello nästa installer.
- Hej RubyInstaller2 för Windows installer startar. Skriv 2 tooinstall hello MSYS2 databasen update. När den är klar och returnerar uppmaning om att installera toohello Stäng hello kommandofönstret.
- Starta en ny kommandotolk (cmd) hello Start-menyn.
- Testa hello Ruby installation `ruby -v` toosee hello versionen installerad.
- Testa installationen av hello symbolen `gem -v` toosee hello versionen installerad.
- Skapa hello PostgreSQL-modul för Ruby med symbolen genom att köra kommandot hello `gem install pg`.

### <a name="macos"></a>MacOS
- Installera Ruby med Homebrew genom att köra kommandot hello `brew install ruby`. Flera installationsalternativ finns hello Ruby [dokumentationen](https://www.ruby-lang.org/en/documentation/installation/#homebrew)
- Testa hello Ruby installation `ruby -v` toosee hello versionen installerad.
- Testa installationen av hello symbolen `gem -v` toosee hello versionen installerad.
- Skapa hello PostgreSQL-modul för Ruby med symbolen genom att köra kommandot hello `gem install pg`.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Installera Ruby genom att köra kommandot hello `sudo apt-get install ruby-full`. Flera installationsalternativ finns hello Ruby [dokumentationen](https://www.ruby-lang.org/en/documentation/installation/).
- Testa hello Ruby installation `ruby -v` toosee hello versionen installerad.
- Installera hello senaste uppdateringarna för symbolen genom att köra kommandot hello `sudo gem update --system`.
- Testa installationen av hello symbolen `gem -v` toosee hello versionen installerad.
- Installera hello gcc, märke och andra verktyg genom att köra kommandot hello `sudo apt-get install build-essential`.
- Installera hello PostgreSQL bibliotek genom att köra kommandot hello `sudo apt-get install libpq-dev`.
- Skapa hello Ruby pg modul med symbolen genom att köra kommandot hello `sudo gem install pg`.

## <a name="run-ruby-code"></a>Köra Ruby-kod 
- Spara hello kod i en textfil och spara hello-filen till en projektmapp med filen tillägget .rb som `C:\rubypostgres\read.rb` eller`/home/username/rubypostgres/read.rb`
- toorun hello kod, starta Kommandotolken hello eller bash shell. Ändra katalog till din projektmapp `cd rubypostgres`, Skriv hello kommando `ruby read.rb` toorun hello program.

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **mypgserver 20170401**.
3. Klicka på servernamnet för hello **mypgserver 20170401**.
4. Välj hello server **översikt** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-ruby/1-connection-string.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet. Om det behövs, Återställ hello lösenord.

## <a name="connect-and-create-a-table"></a>Ansluta och skapa en tabell
Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen, följt av **INSERT INTO** SQL-instruktioner tooadd rader i hello-tabellen.

hello koden använder en [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt med konstruktorn [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure för PostgreSQL-databas. Sedan anropas metoden [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello släpp CREATE TABLE och INSERT INTO-kommandon. hello kod söker efter fel med hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) klass. Sedan anropas metoden [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello anslutningen innan du avslutar.

Ersätt hello `host`, `database`, `user`, och `password` strängar med egna värden. 
```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase'

    # Drop previous table of same name if one exists
    connection.exec('DROP TABLE IF EXISTS inventory;')
    puts 'Finished dropping table (if existed).'

    # Drop previous table of same name if one exists.
    connection.exec('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);')
    puts 'Finished creating table.'

    # Insert some data into table.
    connection.exec("INSERT INTO inventory VALUES(1, 'banana', 150)")
    connection.exec("INSERT INTO inventory VALUES(2, 'orange', 154)")
    connection.exec("INSERT INTO inventory VALUES(3, 'apple', 100)")
    puts 'Inserted 3 rows of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen. 

hello koden använder en [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt med konstruktorn [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure för PostgreSQL-databas. Sedan anropas metoden [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello SELECT-kommandot, hålla hello resultat i en resultatuppsättning. hello resultatet set samlingen är hävdade jämfört med hello `resultSet.each do` loop behålla hello aktuella radvärden i hello `row` variabeln. hello kod söker efter fel med hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) klass. Sedan anropas metoden [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello anslutningen innan du avslutar.

Ersätt hello `host`, `database`, `user`, och `password` strängar med egna värden. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :database => dbname, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase.'

    resultSet = connection.exec('SELECT * from inventory;')
    resultSet.each do |row|
        puts 'Data row = (%s, %s, %s)' % [row['id'], row['name'], row['quantity']]
    end

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.

hello koden använder en [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt med konstruktorn [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure för PostgreSQL-databas. Sedan anropas metoden [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello uppdateringskommando. hello kod söker efter fel med hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) klass. Sedan anropas metoden [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello anslutningen innan du avslutar.

Ersätt hello `host`, `database`, `user`, och `password` strängar med egna värden. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase.'

    # Modify some data in table.
    connection.exec('UPDATE inventory SET quantity = %d WHERE name = %s;' % [200, '\'banana\''])
    puts 'Updated 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```


## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen. 

hello koden använder en [PG::Connection](http://www.rubydoc.info/gems/pg/PG/Connection) objekt med konstruktorn [new()](http://www.rubydoc.info/gems/pg/PG%2FConnection:initialize) tooconnect tooAzure för PostgreSQL-databas. Sedan anropas metoden [exec()](http://www.rubydoc.info/gems/pg/PG/Connection#exec-instance_method) toorun hello uppdateringskommando. hello kod söker efter fel med hello [PG::Error](http://www.rubydoc.info/gems/pg/PG/Error) klass. Sedan anropas metoden [close()](http://www.rubydoc.info/gems/pg/PG/Connection#lo_close-instance_method) tooclose hello anslutningen innan du avslutar.

Ersätt hello `host`, `database`, `user`, och `password` strängar med egna värden. 

```ruby
require 'pg'

begin
    # Initialize connection variables.
    host = String('mypgserver-20170401.postgres.database.azure.com')
    database = String('postgres')
    user = String('mylogin@mypgserver-20170401')
    password = String('<server_admin_password>')

    # Initialize connection object.
    connection = PG::Connection.new(:host => host, :user => user, :dbname => database, :port => '5432', :password => password)
    puts 'Successfully created connection toodatabase.'

    # Modify some data in table.
    connection.exec('DELETE FROM inventory WHERE name = %s;' % ['\'orange\''])
    puts 'Deleted 1 row of data.'

rescue PG::Error => e
    puts e.message 
    
ensure
    connection.close if connection
end
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./howto-migrate-using-export-and-import.md)
