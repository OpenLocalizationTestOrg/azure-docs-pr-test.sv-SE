---
title: "aaaConnect tooAzure databasen för PostgreSQL använder PHP | Microsoft Docs"
description: "Denna Snabbstart innehåller en PHP-kodexempel som du kan använda tooconnect och fråga efter data från Azure-databas för PostgreSQL."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: php
ms.topic: quickstart
ms.date: 06/29/2017
ms.openlocfilehash: 008505e837e37cb8c7fea3fc164b3446c3580e46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-postgresql-use-php-tooconnect-and-query-data"></a>Azure-databas för PostgreSQL: Använd PHP tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure-databas för PostgreSQL med hjälp av en [PHP](http://php.net/manual/intro-whatis.php) program. Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas. Den här artikeln förutsätter att du är bekant med utveckling med hjälp av PHP, men som du är ny tooworking med Azure-databas för PostgreSQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa DB – Portal](quickstart-create-server-database-portal.md)
- [Skapa DB – Azure CLI](quickstart-create-server-database-azure-cli.md)

## <a name="install-php"></a>Installera PHP
Installera PHP på din server, eller skapa en Azure-[webbapp](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) som innehåller PHP.

### <a name="windows"></a>Windows
- Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://windows.php.net/download#php-7.1)
- Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.windows.php) för ytterligare konfiguration
- hello koden använder hello **pgsql** klass (ext/php_pgsql.dll) som ingår i hello PHP-installation. 
- Aktiverade hello **pgsql** tillägg genom att redigera hello php.ini-konfigurationsfilen, normalt finns vid `C:\Program Files\PHP\v7.1\php.ini`. hello konfigurationsfilen ska innehålla en rad med text hello `extension=php_pgsql.so`. Om den inte visas, lägga till hello text och spara hello-filen. Om hello text finns, men kommenterats med ett semikolon prefix, ta bort kommentarerna hello text genom att ta bort hello semikolon.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://php.net/downloads.php) 
- Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.unix.php) för ytterligare konfiguration
- hello koden använder hello **pgsql** klass (php_pgsql.so). Installera den genom att köra `sudo apt-get install php-pgsql`.
- Aktiverade hello **pgsql** tillägg genom att redigera hello `/etc/php/7.0/mods-available/pgsql.ini` konfigurationsfilen. hello konfigurationsfilen ska innehålla en rad med text hello `extension=php_pgsql.so`. Om den inte visas, lägga till hello text och spara hello-filen. Om hello text finns, men kommenterats med ett semikolon prefix, ta bort kommentarerna hello text genom att ta bort hello semikolon.

### <a name="macos"></a>MacOS
- Hämta [PHP 7.1.4](http://php.net/downloads.php)
- Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.macosx.php) för ytterligare konfiguration

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **mypgserver 20170401**.
3. Klicka på servernamnet för hello **mypgserver 20170401**.
4. Välj hello server **översikt** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-php/1-connection-string.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.

## <a name="connect-and-create-a-table"></a>Ansluta och skapa en tabell
Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen, följt av **INSERT INTO** SQL-instruktioner tooadd rader i hello-tabellen.

Hej kod telefonsamtalsmetoden [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure för PostgreSQL-databas. Sedan anropas metoden [pg_query()](http://php.net/manual/en/function.pg-query.php) flera gånger toorun flera kommandon och [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello information om varje gång det uppstod ett fel. Sedan anropas metoden [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello anslutning.

Ersätt hello `$host`, `$database`, `$user`, och `$password` parametrar med egna värden. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";

    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password") 
        or die("Failed toocreate connection toodatabase: ". pg_last_error(). "<br/>");
    print "Successfully created connection toodatabase.<br/>";

    // Drop previous table of same name if one exists.
    $query = "DROP TABLE IF EXISTS inventory;";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");
    print "Finished dropping table (if existed).<br/>";

    // Create table.
    $query = "CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");
    print "Finished creating table.<br/>";

    // Insert some data into table.
    $name = '\'banana\'';
    $quantity = 150;
    $query = "INSERT INTO inventory (name, quantity) VALUES ($1, $2);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");

    $name = '\'orange\'';
    $quantity = 154;
    $query = "INSERT INTO inventory (name, quantity) VALUES ($name, $quantity);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");

    $name = '\'apple\'';
    $quantity = 100;
    $query = "INSERT INTO inventory (name, quantity) VALUES ($name, $quantity);";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error()). "<br/>";

    print "Inserted 3 rows of data.<br/>";

    // Closing connection
    pg_close($connection);
?>
```

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen. 

 Hej kod telefonsamtalsmetoden [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure för PostgreSQL-databas. Sedan anropas metoden [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun hello SELECT-kommandot, hålla hello resultat i en resultatuppsättning och [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello information om ett fel uppstod.  tooread hello resultatmängden metoden [pg_fetch_row()](http://php.net/manual/en/function.pg-fetch-row.php) anropas i en slinga när data som hämtas per rad och hello rad i en matris `$row`, med ett värde per kolumn i varje matris position.  toofree hello resultatmängden metoden [pg_free_result()](http://php.net/manual/en/function.pg-free-result.php) anropas. Sedan anropas metoden [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello anslutning.

Ersätt hello `$host`, `$database`, `$user`, och `$password` parametrar med egna värden. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";
    
    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password")
                or die("Failed toocreate connection toodatabase: ". pg_last_error(). "<br/>");

    print "Successfully created connection toodatabase. <br/>";

    // Perform some SQL queries over hello connection.
    $query = "SELECT * from inventory";
    $result_set = pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). "<br/>");
    while ($row = pg_fetch_row($result_set))
    {
        print "Data row = ($row[0], $row[1], $row[2]). <br/>";
    }

    // Free result_set
    pg_free_result($result_set);

    // Closing connection
    pg_close($connection);
?>
```

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.

Hej kod telefonsamtalsmetoden [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure för PostgreSQL-databas. Sedan anropas metoden [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun ett kommando och [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello information om ett fel uppstod. Sedan anropas metoden [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello anslutning.

Ersätt hello `$host`, `$database`, `$user`, och `$password` parametrar med egna värden. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";

    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password")
                or die("Failed toocreate connection toodatabase: ". pg_last_error(). ".<br/>");

    print "Successfully created connection toodatabase. <br/>";

    // Modify some data in table.
    $new_quantity = 200;
    $name = '\'banana\'';
    $query = "UPDATE inventory SET quantity = $new_quantity WHERE name = $name;";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). ".<br/>");
    print "Updated 1 row of data. </br>";

    // Closing connection
    pg_close($connection);
?>
```


## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen. 

 Hej kod telefonsamtalsmetoden [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect för Azure-databas för PostgreSQL. Sedan anropas metoden [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun ett kommando och [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello information om ett fel uppstod. Sedan anropas metoden [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello anslutning.

Ersätt hello `$host`, `$database`, `$user`, och `$password` parametrar med egna värden. 

```php
<?php
    // Initialize connection variables.
    $host = "mypgserver-20170401.postgres.database.azure.com";
    $database = "mypgsqldb";
    $user = "mylogin@mypgserver-20170401";
    $password = "<server_admin_password>";

    // Initialize connection object.
    $connection = pg_connect("host=$host dbname=$database user=$user password=$password")
            or die("Failed toocreate connection toodatabase: ". pg_last_error(). ". </br>");

    print "Successfully created connection toodatabase. <br/>";

    // Delete some data from table.
    $name = '\'orange\'';
    $query = "DELETE FROM inventory WHERE name = $name;";
    pg_query($connection, $query) 
        or die("Encountered an error when executing given sql statement: ". pg_last_error(). ". <br/>");
    print "Deleted 1 row of data. <br/>";

    // Closing connection
    pg_close($connection);
?>
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./howto-migrate-using-export-and-import.md)
