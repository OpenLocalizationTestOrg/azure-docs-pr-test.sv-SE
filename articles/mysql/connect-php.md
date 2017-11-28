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
# <a name="azure-database-for-mysql-use-php-tooconnect-and-query-data"></a><span data-ttu-id="68922-103">Azure-databas för MySQL: Använd PHP tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="68922-103">Azure Database for MySQL: Use PHP tooconnect and query data</span></span>
<span data-ttu-id="68922-104">Den här snabbstarten visar hur tooconnect tooan Azure-databas för MySQL med hjälp av en [PHP](http://php.net/manual/intro-whatis.php) program.</span><span class="sxs-lookup"><span data-stu-id="68922-104">This quickstart demonstrates how tooconnect tooan Azure Database for MySQL using a [PHP](http://php.net/manual/intro-whatis.php) application.</span></span> <span data-ttu-id="68922-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="68922-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="68922-106">Den här artikeln förutsätter att du är bekant med utveckling med hjälp av PHP, men som du är ny tooworking med Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="68922-106">This article assumes you are familiar with development using PHP, but that you are new tooworking with Azure Database for MySQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68922-107">Krav</span><span class="sxs-lookup"><span data-stu-id="68922-107">Prerequisites</span></span>
<span data-ttu-id="68922-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="68922-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="68922-109">Skapa en Azure Database för MySQL med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="68922-109">Create an Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
- [<span data-ttu-id="68922-110">Skapa en Azure Database för MySQL-server med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="68922-110">Create an Azure Database for MySQL server using Azure CLI</span></span>](./quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-php"></a><span data-ttu-id="68922-111">Installera PHP</span><span class="sxs-lookup"><span data-stu-id="68922-111">Install PHP</span></span>
<span data-ttu-id="68922-112">Installera PHP på din server, eller skapa en Azure-[webbapp](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) som innehåller PHP.</span><span class="sxs-lookup"><span data-stu-id="68922-112">Install PHP on your own server, or create an Azure [web app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) that includes PHP.</span></span>

### <a name="macos"></a><span data-ttu-id="68922-113">MacOS</span><span class="sxs-lookup"><span data-stu-id="68922-113">MacOS</span></span>
- <span data-ttu-id="68922-114">Hämta [PHP 7.1.4](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="68922-114">Download [PHP 7.1.4 version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="68922-115">Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.macosx.php) för ytterligare konfiguration</span><span class="sxs-lookup"><span data-stu-id="68922-115">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.macosx.php) for further configuration</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="68922-116">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="68922-116">Linux (Ubuntu)</span></span>
- <span data-ttu-id="68922-117">Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="68922-117">Download [PHP 7.1.4 non-thread safe (x64) version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="68922-118">Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.unix.php) för ytterligare konfiguration</span><span class="sxs-lookup"><span data-stu-id="68922-118">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.unix.php) for further configuration</span></span>

### <a name="windows"></a><span data-ttu-id="68922-119">Windows</span><span class="sxs-lookup"><span data-stu-id="68922-119">Windows</span></span>
- <span data-ttu-id="68922-120">Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://windows.php.net/download#php-7.1)</span><span class="sxs-lookup"><span data-stu-id="68922-120">Download [PHP 7.1.4 non-thread safe (x64) version](http://windows.php.net/download#php-7.1)</span></span>
- <span data-ttu-id="68922-121">Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.windows.php) för ytterligare konfiguration</span><span class="sxs-lookup"><span data-stu-id="68922-121">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.windows.php) for further configuration</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="68922-122">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="68922-122">Get connection information</span></span>
<span data-ttu-id="68922-123">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för MySQL.</span><span class="sxs-lookup"><span data-stu-id="68922-123">Get hello connection information needed tooconnect toohello Azure Database for MySQL.</span></span> <span data-ttu-id="68922-124">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="68922-124">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="68922-125">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="68922-125">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="68922-126">Hello vänster klickar du på **alla resurser**, och sök sedan efter hello-server som du har skapat (till exempel **myserver4demo**).</span><span class="sxs-lookup"><span data-stu-id="68922-126">In hello left pane, click **All resources**, and then search for hello server you have created (for example, **myserver4demo**).</span></span>
3. <span data-ttu-id="68922-127">Klicka på hello servernamn.</span><span class="sxs-lookup"><span data-stu-id="68922-127">Click hello server name.</span></span>
4. <span data-ttu-id="68922-128">Välj hello server **egenskaper** sidan.</span><span class="sxs-lookup"><span data-stu-id="68922-128">Select hello server's **Properties** page.</span></span> <span data-ttu-id="68922-129">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="68922-129">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="68922-130">![Azure Database för MySQL-servernamn](./media/connect-php/1_server-properties-name-login.png)</span><span class="sxs-lookup"><span data-stu-id="68922-130">![Azure Database for MySQL server name](./media/connect-php/1_server-properties-name-login.png)</span></span>
5. <span data-ttu-id="68922-131">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="68922-131">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="68922-132">Ansluta och skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="68922-132">Connect and create a table</span></span>
<span data-ttu-id="68922-133">Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="68922-133">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement.</span></span> 

<span data-ttu-id="68922-134">hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="68922-134">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="68922-135">Hej kod anropet metoder [mysqli_init](http://php.net/manual/mysqli.init.php) och [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) tooconnect tooMySQL.</span><span class="sxs-lookup"><span data-stu-id="68922-135">hello code call methods [mysqli_init](http://php.net/manual/mysqli.init.php) and [mysqli_real_connect](http://php.net/manual/mysqli.real-connect.php) tooconnect tooMySQL.</span></span> <span data-ttu-id="68922-136">Sedan anropas metoden [mysqli_query](http://php.net/manual/mysqli.query.php) toorun hello frågan.</span><span class="sxs-lookup"><span data-stu-id="68922-136">Then it calls method [mysqli_query](http://php.net/manual/mysqli.query.php) toorun hello query.</span></span> <span data-ttu-id="68922-137">Sedan anropas metoden [mysqli_close](http://php.net/manual/mysqli.close.php) tooclose hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="68922-137">Then it calls method [mysqli_close](http://php.net/manual/mysqli.close.php) tooclose hello connection.</span></span>

<span data-ttu-id="68922-138">Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="68922-138">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="insert-data"></a><span data-ttu-id="68922-139">Infoga data</span><span class="sxs-lookup"><span data-stu-id="68922-139">Insert data</span></span>
<span data-ttu-id="68922-140">Använd hello följande kod tooconnect och infoga data med hjälp av en **infoga** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="68922-140">Use hello following code tooconnect and insert data using an **INSERT** SQL statement.</span></span>

<span data-ttu-id="68922-141">hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="68922-141">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="68922-142">hello används metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate en förberedd Infoga instruktionen sedan bindningar hello parametrar för varje värde i infogade kolumnen med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="68922-142">hello code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate a prepared insert statement, then binds hello parameters for each inserted column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="68922-143">hello koden körs hello-instruktion som använder metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och därefter stängs hello-instruktion som använder metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="68922-143">hello code runs hello statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes hello statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="68922-144">Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="68922-144">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="read-data"></a><span data-ttu-id="68922-145">Läsa data</span><span class="sxs-lookup"><span data-stu-id="68922-145">Read data</span></span>
<span data-ttu-id="68922-146">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="68922-146">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span>  <span data-ttu-id="68922-147">hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="68922-147">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="68922-148">hello används metoden [mysqli_query](http://php.net/manual/mysqli.query.php) utföra hello sql-fråga och använder [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) metoden toofetch hello resulterande rader.</span><span class="sxs-lookup"><span data-stu-id="68922-148">hello code uses method [mysqli_query](http://php.net/manual/mysqli.query.php) perform hello sql query, and uses [mysqli_fetch_assoc](http://php.net/manual/mysqli-result.fetch-assoc.php) method toofetch hello resulting rows.</span></span>

<span data-ttu-id="68922-149">Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="68922-149">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="68922-150">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="68922-150">Update data</span></span>
<span data-ttu-id="68922-151">Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="68922-151">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="68922-152">hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="68922-152">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="68922-153">hello används metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate en förberedd update-instruktion Binder sedan hello parametrar för varje värde i uppdaterade kolumnen med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="68922-153">hello code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate a prepared update statement, then binds hello parameters for each updated column value using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="68922-154">hello koden körs hello-instruktion som använder metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och därefter stängs hello-instruktion som använder metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="68922-154">hello code runs hello statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes hello statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="68922-155">Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="68922-155">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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


## <a name="delete-data"></a><span data-ttu-id="68922-156">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="68922-156">Delete data</span></span>
<span data-ttu-id="68922-157">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="68922-157">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

<span data-ttu-id="68922-158">hello koden använder hello **MySQL förbättrad tillägget** (mysqli)-klass som ingår i PHP.</span><span class="sxs-lookup"><span data-stu-id="68922-158">hello code uses hello **MySQL Improved extension** (mysqli) class included in PHP.</span></span> <span data-ttu-id="68922-159">hello används metoden [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate en förberedd ta bort instruktionen och bindningar hello parametrar för hello där instruktionen i hello-uttrycket med metoden [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span><span class="sxs-lookup"><span data-stu-id="68922-159">hello code uses method [mysqli_prepare](http://php.net/manual/mysqli.prepare.php) toocreate a prepared delete statement, then binds hello parameters for hello where clause in hello statement using method [mysqli_stmt_bind_param](http://php.net/manual/mysqli-stmt.bind-param.php).</span></span> <span data-ttu-id="68922-160">hello koden körs hello-instruktion som använder metoden [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) och därefter stängs hello-instruktion som använder metoden [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span><span class="sxs-lookup"><span data-stu-id="68922-160">hello code runs hello statement using method [mysqli_stmt_execute](http://php.net/manual/mysqli-stmt.execute.php) and afterwards closes hello statement using method [mysqli_stmt_close](http://php.net/manual/mysqli-stmt.close.php).</span></span>

<span data-ttu-id="68922-161">Ersätt hello värden, användarnamn, lösenord och %{db_name/ parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="68922-161">Replace hello host, username, password, and db_name parameters with your own values.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="68922-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68922-162">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="68922-163">Skapa en PHP- och MySQL-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="68922-163">Build a PHP and MySQL web app in Azure</span></span>](../app-service-web/app-service-web-tutorial-php-mysql.md?toc=%2fazure%2fmysql%2ftoc.json)
