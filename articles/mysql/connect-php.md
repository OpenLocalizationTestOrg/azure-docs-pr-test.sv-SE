---
title: "Ansluta till Azure Database för MySQL från PHP | Microsoft Docs"
description: "I den här snabbstarten finns ett kodexempel i PHP som du kan använda för att ansluta till och fråga efter data från Azure Database för MySQL."
services: mysql
author: mswutao
ms.author: wuta
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 07/12/2017
ms.openlocfilehash: 59da1ab9e76685d7ed0c4415ef99578c982e956c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-database-for-mysql-use-php-to-connect-and-query-data"></a><span data-ttu-id="c5d67-103">Azure Database för MySQL: Använda PHP för att ansluta och fråga efter data</span><span class="sxs-lookup"><span data-stu-id="c5d67-103">Azure Database for MySQL: Use PHP to connect and query data</span></span>
<span data-ttu-id="c5d67-104">Den här snabbstarten visar hur du ansluter till en Azure Database för MySQL med hjälp av ett [PHP](http://php.net/manual/intro-whatis.php)-program.</span><span class="sxs-lookup"><span data-stu-id="c5d67-104">This quickstart demonstrates how to connect to an Azure Database for MySQL using a [PHP](http://php.net/manual/intro-whatis.php) application.</span></span> <span data-ttu-id="c5d67-105">Den visar hur du använder SQL-instruktioner för att fråga, infoga, uppdatera och ta bort data i databasen.</span><span class="sxs-lookup"><span data-stu-id="c5d67-105">It shows how to use SQL statements to query, insert, update, and delete data in the database.</span></span> <span data-ttu-id="c5d67-106">Den här artikeln förutsätter att du är van att utveckla i PHP, men saknar erfarenhet av Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="c5d67-106">This article assumes you are familiar with development using PHP, but that you are new to working with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5d67-107">Krav</span><span class="sxs-lookup"><span data-stu-id="c5d67-107">Prerequisites</span></span>
<span data-ttu-id="c5d67-108">I den här snabbstarten används de resurser som skapades i någon av följande guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="c5d67-108">This quickstart uses the resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="c5d67-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c5d67-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="c5d67-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c5d67-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a><span data-ttu-id="c5d67-111">Installera PHP</span><span class="sxs-lookup"><span data-stu-id="c5d67-111">Install PHP</span></span>
<span data-ttu-id="c5d67-112">Installera PHP på din server, eller skapa en Azure-[webbapp](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) som innehåller PHP.</span><span class="sxs-lookup"><span data-stu-id="c5d67-112">Install PHP on your own server, or create an Azure [web app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) that includes PHP.</span></span>

### <a name="macos"></a><span data-ttu-id="c5d67-113">MacOS</span><span class="sxs-lookup"><span data-stu-id="c5d67-113">MacOS</span></span>
- <span data-ttu-id="c5d67-114">Hämta [PHP 7.1.4](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="c5d67-114">Download [PHP 7.1.4 version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="c5d67-115">Installera PHP och se [PHP-handboken](http://php.net/manual/install.macosx.php) för ytterligare konfiguration</span><span class="sxs-lookup"><span data-stu-id="c5d67-115">Install PHP and refer to the [PHP manual](http://php.net/manual/install.macosx.php) for further configuration</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="c5d67-116">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="c5d67-116">Linux (Ubuntu)</span></span>
- <span data-ttu-id="c5d67-117">Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="c5d67-117">Download [PHP 7.1.4 non-thread safe (x64) version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="c5d67-118">Installera PHP och se [PHP-handboken](http://php.net/manual/install.unix.php) för ytterligare konfiguration</span><span class="sxs-lookup"><span data-stu-id="c5d67-118">Install PHP and refer to the [PHP manual](http://php.net/manual/install.unix.php) for further configuration</span></span>

### <a name="windows"></a><span data-ttu-id="c5d67-119">Windows</span><span class="sxs-lookup"><span data-stu-id="c5d67-119">Windows</span></span>
- <span data-ttu-id="c5d67-120">Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://windows.php.net/download#php-7.1)</span><span class="sxs-lookup"><span data-stu-id="c5d67-120">Download [PHP 7.1.4 non-thread safe (x64) version](http://windows.php.net/download#php-7.1)</span></span>
- <span data-ttu-id="c5d67-121">Installera PHP och se [PHP-handboken](http://php.net/manual/install.windows.php) för ytterligare konfiguration</span><span class="sxs-lookup"><span data-stu-id="c5d67-121">Install PHP and refer to the [PHP manual](http://php.net/manual/install.windows.php) for further configuration</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="c5d67-122">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="c5d67-122">Get connection information</span></span>
<span data-ttu-id="c5d67-123">Skaffa den information som du behöver för att ansluta till Azure Database för MySQL.</span><span class="sxs-lookup"><span data-stu-id="c5d67-123">Get the connection information needed to connect to the Azure Database for MySQL.</span></span> <span data-ttu-id="c5d67-124">Du behöver det fullständiga servernamnet och inloggningsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="c5d67-124">You need the fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="c5d67-125">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="c5d67-125">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="c5d67-126">I den vänstra rutan klickar du på **alla resurser** och söker sedan efter servern som du skapade (till exempel **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="c5d67-126">In the left pane, click **All resources**, and then search for the server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="c5d67-127">Klicka på servernamnet.</span><span class="sxs-lookup"><span data-stu-id="c5d67-127">Click the server name.</span></span>
4. <span data-ttu-id="c5d67-128">Välj sidan **Egenskaper** för servern.</span><span class="sxs-lookup"><span data-stu-id="c5d67-128">Select the server's **Properties** page.</span></span> <span data-ttu-id="c5d67-129">Anteckna **servernamn** och **inloggningsnamnet för serveradministratören**.</span><span class="sxs-lookup"><span data-stu-id="c5d67-129">Make a note of the **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="c5d67-130">![Azure Database för MySQL-servernamn](./media/connect-php/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="c5d67-130">![Azure Database for MySQL server name](./media/connect-php/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="c5d67-131">Om du glömmer inloggningsinformationen för servern öppnar du sidan **Översikt** för att se inloggningsnamnet för serveradministratören. Om det behövs kan du återställa lösenordet.</span><span class="sxs-lookup"><span data-stu-id="c5d67-131">If you forget your server login information, navigate to the **Overview** page to view the Server admin login name and, if necessary, reset the password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="c5d67-132">Ansluta och skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="c5d67-132">Connect and create a table</span></span>
<span data-ttu-id="c5d67-133">Använd följande kod för att ansluta och skapa en tabell med hjälp av SQL-instruktionen **CREATE TABLE**.</span><span class="sxs-lookup"><span data-stu-id="c5d67-133">Use the following code to connect and create a table using **CREATE TABLE** SQL statement.</span></span> 

<span data-ttu-id="c5d67-134">Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="c5d67-134">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c5d67-135">Metoderna [mysqli_init](http://php.net/manual/mysqli.init.php) och [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) anropas för att ansluta till MySQL.</span><span class="sxs-lookup"><span data-stu-id="c5d67-135">The code call methods [mysqli_init](http://php.net/manual/mysqli.init.php) and [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) to connect to MySQL.</span></span> <span data-ttu-id="c5d67-136">Sedan anropas metoden [mysqli_query](http://php.net/manual/mysqli.query.php) för att köra frågan.</span><span class="sxs-lookup"><span data-stu-id="c5d67-136">Then it calls method [mysqli_query](http://php.net/manual/mysqli.query.php) to run the query.</span></span> <span data-ttu-id="c5d67-137">Sedan anropas metoden [mysqli_close](http://php.net/manual/mysqli.close.php) för att stänga anslutningen.</span><span class="sxs-lookup"><span data-stu-id="c5d67-137">Then it calls method [mysqli_close](http://php.net/manual/mysqli.close.php) to close the connection.</span></span>

<span data-ttu-id="c5d67-138">Ersätt parametrarna host, username, password och db_name med dina egna värden.</span><span class="sxs-lookup"><span data-stu-id="c5d67-138">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="insert-data"></a><span data-ttu-id="c5d67-139">Infoga data</span><span class="sxs-lookup"><span data-stu-id="c5d67-139">Insert data</span></span>
<span data-ttu-id="c5d67-140">Använd följande kod för att ansluta och infoga data med en **INSERT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c5d67-140">Use the following code to connect and insert data using an **INSERT** SQL statement.</span></span>

<span data-ttu-id="c5d67-141">Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="c5d67-141">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c5d67-142">I koden används metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) för att skapa en förberedd insert-instruktion och sedan binds parametrarna för varje infogat kolumnvärde med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="c5d67-142">The code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) to create a prepared insert statement, then binds the parameters for each inserted column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="c5d67-143">Sedan körs instruktionen med metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och efteråt stängs instruktionen med metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="c5d67-143">The code runs the statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes the statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="c5d67-144">Ersätt parametrarna host, username, password och db_name med dina egna värden.</span><span class="sxs-lookup"><span data-stu-id="c5d67-144">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="read-data"></a><span data-ttu-id="c5d67-145">Läsa data</span><span class="sxs-lookup"><span data-stu-id="c5d67-145">Read data</span></span>
<span data-ttu-id="c5d67-146">Använd följande kod för att ansluta och läsa data med en **SELECT**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c5d67-146">Use the following code to connect and read the data using a **SELECT** SQL statement.</span></span>  <span data-ttu-id="c5d67-147">Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="c5d67-147">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c5d67-148">Metoden [mysqli_query](http://php.net/manual/mysqli.query.php) används för att utföra sql-frågan och metoden [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) används för att hämta de resulterande raderna.</span><span class="sxs-lookup"><span data-stu-id="c5d67-148">The code uses method [mysqli_query](http://php.net/manual/mysqli.query.php) perform the sql query, and uses [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) method to fetch the resulting rows.</span></span>

<span data-ttu-id="c5d67-149">Ersätt parametrarna host, username, password och db_name med dina egna värden.</span><span class="sxs-lookup"><span data-stu-id="c5d67-149">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="c5d67-150">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="c5d67-150">Update data</span></span>
<span data-ttu-id="c5d67-151">Använd följande kod för att ansluta och uppdatera data med en **UPDATE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c5d67-151">Use the following code to connect and update the data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="c5d67-152">Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="c5d67-152">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c5d67-153">Metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) används för att skapa en förberedd update-instruktion och sedan binds parametrarna för varje uppdaterat kolumnvärde med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="c5d67-153">The code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) to create a prepared update statement, then binds the parameters for each updated column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="c5d67-154">Sedan körs instruktionen med metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och efteråt stängs instruktionen med metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="c5d67-154">The code runs the statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes the statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="c5d67-155">Ersätt parametrarna host, username, password och db_name med dina egna värden.</span><span class="sxs-lookup"><span data-stu-id="c5d67-155">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

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


## <a name="delete-data"></a><span data-ttu-id="c5d67-156">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="c5d67-156">Delete data</span></span>
<span data-ttu-id="c5d67-157">Använd följande kod för att ansluta och läsa data med en **DELETE**-SQL-instruktion.</span><span class="sxs-lookup"><span data-stu-id="c5d67-157">Use the following code to connect and read the data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="c5d67-158">Koden använder klassen **MySQL Improved extension** (mysqli) som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="c5d67-158">The code uses the **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="c5d67-159">Metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) används för att skapa en förberedd delete-instruktion och sedan binds parametrarna för where-satsen med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="c5d67-159">The code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) to create a prepared delete statement, then binds the parameters for the where clause in the statement using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="c5d67-160">Sedan körs instruktionen med metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och efteråt stängs instruktionen med metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="c5d67-160">The code runs the statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes the statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="c5d67-161">Ersätt parametrarna host, username, password och db_name med dina egna värden.</span><span class="sxs-lookup"><span data-stu-id="c5d67-161">Replace the host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="c5d67-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5d67-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c5d67-163">Skapa en PHP- och MySQL-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="c5d67-163">Build a PHP and MySQL web app in Azure</span></span>](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
