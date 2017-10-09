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
# <a name="azure-database-for-postgresql-use-php-tooconnect-and-query-data"></a><span data-ttu-id="ca6bd-103">Azure-databas för PostgreSQL: Använd PHP tooconnect och fråga data</span><span class="sxs-lookup"><span data-stu-id="ca6bd-103">Azure Database for PostgreSQL: Use PHP tooconnect and query data</span></span>
<span data-ttu-id="ca6bd-104">Den här snabbstarten visar hur tooconnect tooan Azure-databas för PostgreSQL med hjälp av en [PHP](http://php.net/manual/intro-whatis.php) program.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-104">This quickstart demonstrates how tooconnect tooan Azure Database for PostgreSQL using a [PHP](http://php.net/manual/intro-whatis.php) application.</span></span> <span data-ttu-id="ca6bd-105">Den visar hur toouse SQL-instruktioner tooquery infoga, uppdatera och ta bort data i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-105">It shows how toouse SQL statements tooquery, insert, update, and delete data in hello database.</span></span> <span data-ttu-id="ca6bd-106">Den här artikeln förutsätter att du är bekant med utveckling med hjälp av PHP, men som du är ny tooworking med Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-106">This article assumes you are familiar with development using PHP, but that you are new tooworking with Azure Database for PostgreSQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca6bd-107">Krav</span><span class="sxs-lookup"><span data-stu-id="ca6bd-107">Prerequisites</span></span>
<span data-ttu-id="ca6bd-108">Denna Snabbstart använder hello resurser som skapades i någon av dessa guider som utgångspunkt:</span><span class="sxs-lookup"><span data-stu-id="ca6bd-108">This quickstart uses hello resources created in either of these guides as a starting point:</span></span>
- [<span data-ttu-id="ca6bd-109">Skapa DB – Portal</span><span class="sxs-lookup"><span data-stu-id="ca6bd-109">Create DB - Portal</span></span>](quickstart-create-server-database-portal.md)
- [<span data-ttu-id="ca6bd-110">Skapa DB – Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ca6bd-110">Create DB - Azure CLI</span></span>](quickstart-create-server-database-azure-cli.md)

## <a name="install-php"></a><span data-ttu-id="ca6bd-111">Installera PHP</span><span class="sxs-lookup"><span data-stu-id="ca6bd-111">Install PHP</span></span>
<span data-ttu-id="ca6bd-112">Installera PHP på din server, eller skapa en Azure-[webbapp](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) som innehåller PHP.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-112">Install PHP on your own server, or create an Azure [web app](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-overview) that includes PHP.</span></span>

### <a name="windows"></a><span data-ttu-id="ca6bd-113">Windows</span><span class="sxs-lookup"><span data-stu-id="ca6bd-113">Windows</span></span>
- <span data-ttu-id="ca6bd-114">Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://windows.php.net/download#php-7.1)</span><span class="sxs-lookup"><span data-stu-id="ca6bd-114">Download [PHP 7.1.4 non-thread safe (x64) version](http://windows.php.net/download#php-7.1)</span></span>
- <span data-ttu-id="ca6bd-115">Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.windows.php) för ytterligare konfiguration</span><span class="sxs-lookup"><span data-stu-id="ca6bd-115">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.windows.php) for further configuration</span></span>
- <span data-ttu-id="ca6bd-116">hello koden använder hello **pgsql** klass (ext/php_pgsql.dll) som ingår i hello PHP-installation.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-116">hello code uses hello **pgsql** class (ext/php_pgsql.dll)  that is included in hello PHP installation.</span></span> 
- <span data-ttu-id="ca6bd-117">Aktiverade hello **pgsql** tillägg genom att redigera hello php.ini-konfigurationsfilen, normalt finns vid `C:\Program Files\PHP\v7.1\php.ini`.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-117">Enabled hello **pgsql** extension by editing hello php.ini configuration file, typically located at `C:\Program Files\PHP\v7.1\php.ini`.</span></span> <span data-ttu-id="ca6bd-118">hello konfigurationsfilen ska innehålla en rad med text hello `extension=php_pgsql.so`.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-118">hello configuration file should contain a line with hello text `extension=php_pgsql.so`.</span></span> <span data-ttu-id="ca6bd-119">Om den inte visas, lägga till hello text och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-119">If it is not shown, add hello text and save hello file.</span></span> <span data-ttu-id="ca6bd-120">Om hello text finns, men kommenterats med ett semikolon prefix, ta bort kommentarerna hello text genom att ta bort hello semikolon.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-120">If hello text is present, but commented with a semicolon prefix, uncomment hello text by removing hello semicolon.</span></span>

### <a name="linux-ubuntu"></a><span data-ttu-id="ca6bd-121">Linux (Ubuntu)</span><span class="sxs-lookup"><span data-stu-id="ca6bd-121">Linux (Ubuntu)</span></span>
- <span data-ttu-id="ca6bd-122">Hämta [PHP 7.1.4 (x64), ej trådsäker version](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="ca6bd-122">Download [PHP 7.1.4 non-thread safe (x64) version](http://php.net/downloads.php)</span></span> 
- <span data-ttu-id="ca6bd-123">Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.unix.php) för ytterligare konfiguration</span><span class="sxs-lookup"><span data-stu-id="ca6bd-123">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.unix.php) for further configuration</span></span>
- <span data-ttu-id="ca6bd-124">hello koden använder hello **pgsql** klass (php_pgsql.so).</span><span class="sxs-lookup"><span data-stu-id="ca6bd-124">hello code uses hello **pgsql** class (php_pgsql.so).</span></span> <span data-ttu-id="ca6bd-125">Installera den genom att köra `sudo apt-get install php-pgsql`.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-125">Install it by running `sudo apt-get install php-pgsql`.</span></span>
- <span data-ttu-id="ca6bd-126">Aktiverade hello **pgsql** tillägg genom att redigera hello `/etc/php/7.0/mods-available/pgsql.ini` konfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-126">Enabled hello **pgsql** extension by editing hello `/etc/php/7.0/mods-available/pgsql.ini` configuration file.</span></span> <span data-ttu-id="ca6bd-127">hello konfigurationsfilen ska innehålla en rad med text hello `extension=php_pgsql.so`.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-127">hello configuration file should contain a line with hello text `extension=php_pgsql.so`.</span></span> <span data-ttu-id="ca6bd-128">Om den inte visas, lägga till hello text och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-128">If it is not shown, add hello text and save hello file.</span></span> <span data-ttu-id="ca6bd-129">Om hello text finns, men kommenterats med ett semikolon prefix, ta bort kommentarerna hello text genom att ta bort hello semikolon.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-129">If hello text is present, but commented with a semicolon prefix, uncomment hello text by removing hello semicolon.</span></span>

### <a name="macos"></a><span data-ttu-id="ca6bd-130">MacOS</span><span class="sxs-lookup"><span data-stu-id="ca6bd-130">MacOS</span></span>
- <span data-ttu-id="ca6bd-131">Hämta [PHP 7.1.4](http://php.net/downloads.php)</span><span class="sxs-lookup"><span data-stu-id="ca6bd-131">Download [PHP 7.1.4 version](http://php.net/downloads.php)</span></span>
- <span data-ttu-id="ca6bd-132">Installera PHP och finns toohello [PHP-handboken](http://php.net/manual/install.macosx.php) för ytterligare konfiguration</span><span class="sxs-lookup"><span data-stu-id="ca6bd-132">Install PHP and refer toohello [PHP manual](http://php.net/manual/install.macosx.php) for further configuration</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="ca6bd-133">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="ca6bd-133">Get connection information</span></span>
<span data-ttu-id="ca6bd-134">Hämta hello anslutning information som behövs för tooconnect toohello Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-134">Get hello connection information needed tooconnect toohello Azure Database for PostgreSQL.</span></span> <span data-ttu-id="ca6bd-135">Du måste hello server fullständigt kvalificerade namnet och autentiseringsuppgifterna för inloggning.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-135">You need hello fully qualified server name and login credentials.</span></span>

1. <span data-ttu-id="ca6bd-136">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ca6bd-136">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="ca6bd-137">Hello vänstra menyn i Azure-portalen klickar du på **alla resurser** och Sök efter hello-server som du har skapat, exempelvis **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-137">From hello left-hand menu in Azure portal, click **All resources** and search for hello server you have created, such as **mypgserver-20170401**.</span></span>
3. <span data-ttu-id="ca6bd-138">Klicka på servernamnet för hello **mypgserver 20170401**.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-138">Click hello server name **mypgserver-20170401**.</span></span>
4. <span data-ttu-id="ca6bd-139">Välj hello server **översikt** sidan.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-139">Select hello server's **Overview** page.</span></span> <span data-ttu-id="ca6bd-140">Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-140">Make a note of hello **Server name** and **Server admin login name**.</span></span>
 <span data-ttu-id="ca6bd-141">![Azure Database för PostgreSQL – inloggning för serveradministratör](./media/connect-php/1-connection-string.png)</span><span class="sxs-lookup"><span data-stu-id="ca6bd-141">![Azure Database for PostgreSQL - Server Admin Login](./media/connect-php/1-connection-string.png)</span></span>
5. <span data-ttu-id="ca6bd-142">Om du glömmer bort inloggningsinformationen server navigera toohello **översikt** sidan tooview hello admin serverinloggningsnamnet och, om nödvändigt återställa hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-142">If you forget your server login information, navigate toohello **Overview** page tooview hello Server admin login name and, if necessary, reset hello password.</span></span>

## <a name="connect-and-create-a-table"></a><span data-ttu-id="ca6bd-143">Ansluta och skapa en tabell</span><span class="sxs-lookup"><span data-stu-id="ca6bd-143">Connect and create a table</span></span>
<span data-ttu-id="ca6bd-144">Använd hello följande kod tooconnect och skapa en tabell med hjälp av **CREATE TABLE** SQL-instruktionen, följt av **INSERT INTO** SQL-instruktioner tooadd rader i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-144">Use hello following code tooconnect and create a table using **CREATE TABLE** SQL statement, followed by **INSERT INTO** SQL statements tooadd rows into hello table.</span></span>

<span data-ttu-id="ca6bd-145">Hej kod telefonsamtalsmetoden [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure för PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-145">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="ca6bd-146">Sedan anropas metoden [pg_query()](http://php.net/manual/en/function.pg-query.php) flera gånger toorun flera kommandon och [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello information om varje gång det uppstod ett fel.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-146">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) several times toorun several commands, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred each time.</span></span> <span data-ttu-id="ca6bd-147">Sedan anropas metoden [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-147">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="ca6bd-148">Ersätt hello `$host`, `$database`, `$user`, och `$password` parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-148">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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

## <a name="read-data"></a><span data-ttu-id="ca6bd-149">Läsa data</span><span class="sxs-lookup"><span data-stu-id="ca6bd-149">Read data</span></span>
<span data-ttu-id="ca6bd-150">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **Välj** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-150">Use hello following code tooconnect and read hello data using a **SELECT** SQL statement.</span></span> 

 <span data-ttu-id="ca6bd-151">Hej kod telefonsamtalsmetoden [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure för PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-151">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="ca6bd-152">Sedan anropas metoden [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun hello SELECT-kommandot, hålla hello resultat i en resultatuppsättning och [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello information om ett fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-152">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun hello SELECT command, keeping hello results in a result set, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred.</span></span>  <span data-ttu-id="ca6bd-153">tooread hello resultatmängden metoden [pg_fetch_row()](http://php.net/manual/en/function.pg-fetch-row.php) anropas i en slinga när data som hämtas per rad och hello rad i en matris `$row`, med ett värde per kolumn i varje matris position.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-153">tooread hello result set, method [pg_fetch_row()](http://php.net/manual/en/function.pg-fetch-row.php) is called in a loop, once per row, and hello row data is retrieved in an array `$row`, with one data value per column in each array position.</span></span>  <span data-ttu-id="ca6bd-154">toofree hello resultatmängden metoden [pg_free_result()](http://php.net/manual/en/function.pg-free-result.php) anropas.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-154">toofree hello result set, method [pg_free_result()](http://php.net/manual/en/function.pg-free-result.php) is called.</span></span> <span data-ttu-id="ca6bd-155">Sedan anropas metoden [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-155">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="ca6bd-156">Ersätt hello `$host`, `$database`, `$user`, och `$password` parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-156">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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

## <a name="update-data"></a><span data-ttu-id="ca6bd-157">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="ca6bd-157">Update data</span></span>
<span data-ttu-id="ca6bd-158">Använd hello följande kod tooconnect och uppdatera hello data med hjälp av en **uppdatera** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-158">Use hello following code tooconnect and update hello data using a **UPDATE** SQL statement.</span></span>

<span data-ttu-id="ca6bd-159">Hej kod telefonsamtalsmetoden [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure för PostgreSQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-159">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect tooAzure Database for PostgreSQL.</span></span> <span data-ttu-id="ca6bd-160">Sedan anropas metoden [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun ett kommando och [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello information om ett fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-160">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun a command, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred.</span></span> <span data-ttu-id="ca6bd-161">Sedan anropas metoden [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-161">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="ca6bd-162">Ersätt hello `$host`, `$database`, `$user`, och `$password` parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-162">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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


## <a name="delete-data"></a><span data-ttu-id="ca6bd-163">Ta bort data</span><span class="sxs-lookup"><span data-stu-id="ca6bd-163">Delete data</span></span>
<span data-ttu-id="ca6bd-164">Använd hello följande kod tooconnect och läsa hello data med hjälp av en **ta bort** SQL-instruktionen.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-164">Use hello following code tooconnect and read hello data using a **DELETE** SQL statement.</span></span> 

 <span data-ttu-id="ca6bd-165">Hej kod telefonsamtalsmetoden [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect för Azure-databas för PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-165">hello code call method [pg_connect()](http://php.net/manual/en/function.pg-connect.php) tooconnect too Azure Database for PostgreSQL.</span></span> <span data-ttu-id="ca6bd-166">Sedan anropas metoden [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun ett kommando och [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello information om ett fel uppstod.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-166">Then it calls method [pg_query()](http://php.net/manual/en/function.pg-query.php) toorun a command, and [pg_last_error()](http://php.net/manual/en/function.pg-last-error.php) toocheck hello details if an error occurred.</span></span> <span data-ttu-id="ca6bd-167">Sedan anropas metoden [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-167">Then it calls method [pg_close()](http://php.net/manual/en/function.pg-close.php) tooclose hello connection.</span></span>

<span data-ttu-id="ca6bd-168">Ersätt hello `$host`, `$database`, `$user`, och `$password` parametrar med egna värden.</span><span class="sxs-lookup"><span data-stu-id="ca6bd-168">Replace hello `$host`, `$database`, `$user`, and `$password` parameters with your own values.</span></span> 

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

## <a name="next-steps"></a><span data-ttu-id="ca6bd-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ca6bd-169">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ca6bd-170">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="ca6bd-170">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
