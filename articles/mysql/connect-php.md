---
title: "Ansluta till Azure Database för MySQL från PHP | Microsoft Docs"
description: "I den här snabbstarten finns ett kodexempel i PHP som du kan använda för att ansluta till och fråga efter data från Azure Database för MySQL."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql
ms.custom: mvc
ms.topic: quickstart
ms.date: 09/22/2017
ms.openlocfilehash: 530b02e7cd2a45ce3d9b8968b090d2b48b34e843
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/03/2018
---
# <a name="azure-database-for-mysql-use-php-to-connect-and-query-data"></a>Azure Database för MySQL: Använda PHP för att ansluta och fråga efter data
Den här snabbstarten visar hur du ansluter till en Azure Database för MySQL med hjälp av ett [PHP](http://php.net/manual/intro-whatis.php)-program. Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen. Det här avsnittet förutsätter att du är van att utveckla i PHP och att du saknar erfarenhet av Azure Database for MySQL.

## <a name="prerequisites"></a>Nödvändiga komponenter
I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:
- [Skapa en Azure Database för MySQL med Azure Portal](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [Skapa en Azure Database för MySQL-server med Azure CLI](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a>Installera PHP
Installera PHP på din server, eller skapa en Azure-[webbapp](../app-service/app-service-web-overview.md) som innehåller PHP.

### <a name="macos"></a>MacOS
- Hämta [PHP 7.1.4](http://php.net/downloads.php).
- Installera PHP och se [PHP-handboken](http://php.net/manual/install.macosx.php) för ytterligare konfiguration.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)
- Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://php.net/downloads.php).
- Installera PHP och se [PHP-handboken](http://php.net/manual/install.unix.php) för ytterligare konfiguration.

### <a name="windows"></a>Windows
- Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://windows.php.net/download#php-7.1).
- Installera PHP och se [PHP-handboken](http://php.net/manual/install.windows.php) för ytterligare konfiguration.

## <a name="get-connection-information"></a>Hämta anslutningsinformation
Skaffa den information som du behöver för att ansluta till Azure Database för MySQL. Du behöver det fullständiga servernamnet och inloggningsuppgifter.

1. Logga in på [Azure-portalen](https://portal.azure.com/).
2. I den vänstra rutan klickar du på **alla resurser** och söker sedan efter servern som du skapade (till exempel **myserver4demo**).
3. Klicka på servernamnet.
4. Välj sidan **Egenskaper** för servern och notera **Servernamn** och **Inloggningsnamn för serveradministratören**.
 ![Azure Database för MySQL-servernamn](./media/connect-php/1_server-properties-name-login.png)
5. Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.

## <a name="connect-and-create-a-table"></a>Ansluta och skapa en tabell
Använd följande kod för att ansluta och skapa en tabell med hjälp av SQL-instruktionen **CREATE TABLE**. 

Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP. Metoderna [mysqli_init](http://php.net/manual/mysqli.init.php) och [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) anropas för att ansluta till MySQL. Sedan anropas metoden [mysqli_query](http://php.net/manual/mysqli.query.php) för att köra frågan. Sedan anropas metoden [mysqli_close](http://php.net/manual/mysqli.close.php) för att stänga anslutningen.

Ersätt parametrarna host, username, password och db_name med dina egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

// Run the create table query
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

//Close the connection
mysqli_close($conn);
?>
```

## <a name="insert-data"></a>Infoga data
Använd följande kod för att ansluta och infoga data med en **INSERT**-SQL-instruktion.

Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP. I koden används metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) för att skapa en förberedd insert-instruktion och sedan binds parametrarna för varje infogat kolumnvärde med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Sedan körs instruktionen med metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och efteråt stängs instruktionen med metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Ersätt parametrarna host, username, password och db_name med dina egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
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

// Close the connection
mysqli_close($conn);
?>
```

## <a name="read-data"></a>Läsa data
Använd följande kod för att ansluta och läsa data med SQL-instruktionen **SELECT**.  Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP. Metoden [mysqli_query](http://php.net/manual/mysqli.query.php) används för att utföra sql-frågan och metoden [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) används för att hämta de resulterande raderna.

Ersätt parametrarna host, username, password och db_name med dina egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Select query
printf("Reading data from table: \n");
$res = mysqli_query($conn, 'SELECT * FROM Products');
while ($row = mysqli_fetch_assoc($res)) {
var_dump($row);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="update-data"></a>Uppdatera data
Använd följande kod för att ansluta och uppdatera data med en **UPDATE**-SQL-instruktion.

Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP. Metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) används för att skapa en förberedd update-instruktion och sedan binds parametrarna för varje uppdaterat kolumnvärde med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Sedan körs instruktionen med metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och efteråt stängs instruktionen med metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Ersätt parametrarna host, username, password och db_name med dina egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Update statement
$product_name = 'BrandNewProduct';
$new_product_price = 15.1;
if ($stmt = mysqli_prepare($conn, "UPDATE Products SET Price = ? WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 'ds', $new_product_price, $product_name);
mysqli_stmt_execute($stmt);
printf("Update: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));

//Close the connection
mysqli_stmt_close($stmt);
}

mysqli_close($conn);
?>
```


## <a name="delete-data"></a>Ta bort data
Använd följande kod för att ansluta och läsa data med SQL-instruktionen **DELETE**. 

Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP. Metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) används för att skapa en förberedd delete-instruktion och sedan binds parametrarna för where-satsen med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php). Sedan körs instruktionen med metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och efteråt stängs instruktionen med metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).

Ersätt parametrarna host, username, password och db_name med dina egna värden. 

```php
<?php
$host = 'myserver4demo.mysql.database.azure.com';
$username = 'myadmin@myserver4demo';
$password = 'your_password';
$db_name = 'your_database';

//Establishes the connection
$conn = mysqli_init();
mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}

//Run the Delete statement
$product_name = 'BrandNewProduct';
if ($stmt = mysqli_prepare($conn, "DELETE FROM Products WHERE ProductName = ?")) {
mysqli_stmt_bind_param($stmt, 's', $product_name);
mysqli_stmt_execute($stmt);
printf("Delete: Affected %d rows\n", mysqli_stmt_affected_rows($stmt));
mysqli_stmt_close($stmt);
}

//Close the connection
mysqli_close($conn);
?>
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Ansluta till Azure Database for MySQL via SSL] (howto-configure-ssl.md)
