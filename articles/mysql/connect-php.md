---
title: "Ansluta tooAzure databas för MySQL från PHP | Microsoft Docs"
description: "Denna Snabbstart innehåller flera PHP-kodexempel du kan använda tooconnect och fråga efter data från Azure-databas för MySQL."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 07/12/2017
ms.openlocfilehash: b928748c473c1aef320ae2183f237b5b845e83f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-database-for-mysql-use-php-tooconnect-and-query-data"></a>Azure-databas för MySQL: Använd PHP tooconnect och fråga data
Den här snabbstarten visar hur tooconnect tooan Azure-databas för MySQL med hjälp av en [PHP](http://php.net/manual/intro-whatis.php) program. Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas. Den här artikeln förutsätter att du är bekant med utveckling med hjälp av PHP, men som du är ny tooworking med Azure-databas för MySQL.

## <a name="prerequisites"></a>Krav
Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:
- [Skapa en Azure Database för MySQL med Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Skapa en Azure Database för MySQL-server med Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a>Installera PHP
Installera PHP på din server, eller skapa en Azure-[webbapp](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) som innehåller PHP.

### <a name="macos"></a>MacOS
- Hämta [PHP 7.1.4](http://php.net/downloads.php)
- Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.macosx.php) för ytterligare konfiguration

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://php.net/downloads.php)
- Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.unix.php) för ytterligare konfiguration

### <a name="windows"></a>Windows
- Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://windows.php.net/download#php-7.1)
- Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.windows.php) för ytterligare konfiguration

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL. Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hello vänster klickar du på **alla resurser**, och sök sedan efter hello-server som du har skapat (till exempel **myserver4demo**).
3. Klicka på hello servernamn.
4. Välj hello server **egenskaper** sidan. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.
 ![Azure Database för MySQL-servernamn](./media/connect-php/1_server-properties-name-login.png)
5. Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.

## <a name="connect-and-create-a-table"></a>Ansluta och skapa en tabell
Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen. 

hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP. Hej kod anropet metoder [mysqli_init](http://php.net/manual/mysqli.init.php) och [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) tooconnect tooMySQL. Sedan anropas metoden [mysqli_query](http://php.net/manual/mysqli.query.php) toorun hello frågan. Sedan anropas metoden [mysqli_close](http://php.net/manual/mysqli.close.php) tooclose hello anslutning.

Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

// Run hello create table query
if (mysqli_query($conn, '
CREATE TABLE Products (
`Id` INT NOT NULL AUTO_INCREMENT ,
`ProductName` VARCHAR(200) NOT NULL ,
`Color` VARCHAR(50) NOT NULL ,
`Price` DOUBLE NOT NULL ,
PRIMARY KEY (`Id`)
);
')) {
printf("Table created\n");
}

//Close hello connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a>Infoga data
Använd hello följande kod tooconnect och infoga data med hjälp av en **infoga** SQL-instruktionen.

hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP. hello används metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate en förberedd Infoga instruktionen sedan bindningar hello parametrar för varje värde i infogade kolumnen med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). hello koden körs hello-instruktion som använder metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och därefter stängs hello-instruktion som använder metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Create an Insert prepared statement and run it
$product_name = 'BrandNewProduct';
$product_color = 'Blue';
$product_price = 15.5;
if ($stmt = mysqli_prepare($conn, "INSERT INTO Products (ProductName, Color, Price) VALUES (?, ?, ?)")) {
mysqli_stmt_bind_param($stmt, 'ssd', $product_name, $product_color, $product_price);
mysqli_stmt_execute($stmt);
printf("Insert: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

// Close hello connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a>Läsa data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.  hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP. hello används metoden [mysqli_query](http://php.net/manual/mysqli.query.php) utföra hello sql-fråga och använder [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) metoden toofetch hello resulterande rader.

Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Run hello Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close hello connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a>Uppdatera data
Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.

hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP. hello används metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate en förberedd update-instruktion Binder sedan hello parametrar för varje värde i uppdaterade kolumnen med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). hello koden körs hello-instruktion som använder metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och därefter stängs hello-instruktion som använder metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Run hello Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close hello connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a>Ta bort data
Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen. 

hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP. hello används metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate en förberedd ta bort instruktionen och bindningar hello parametrar för hello där instruktionen i hello-uttrycket med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). hello koden körs hello-instruktion som använder metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och därefter stängs hello-instruktion som använder metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes hello connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}

//Run hello Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close hello connection
mysqli_close($conn);
?>
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Skapa en PHP- och MySQL-webbapp i Azure](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
