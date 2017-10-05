---
title: Skapa en PHP- och MySQL-webbapp i Azure | Microsoft Docs
description: "Lär dig hur du hämtar en PHP-app som arbetar i Azure, med anslutning till en MySQL-databas i Azure."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 14feb4f3-5095-496e-9a40-690e1414bd73
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 07/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 6e8d8962180f7534b0b9074f03ecc8ea431ae1a4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a><span data-ttu-id="e7d4b-103">Skapa en PHP- och MySQL-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-103">Build a PHP and MySQL web app in Azure</span></span>

<span data-ttu-id="e7d4b-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="e7d4b-105">Den här kursen visar hur du skapar en PHP-webbapp i Azure och ansluta till en MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-105">This tutorial shows how to create a PHP web app in Azure and connect it to a MySQL database.</span></span> <span data-ttu-id="e7d4b-106">När du är klar har du en [Laravel](https://laravel.com/) app som körs på Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-106">When you're finished, you'll have a [Laravel](https://laravel.com/) app running on Azure App Service Web Apps.</span></span>

![PHP-app som körs i Azure App Service](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="e7d4b-108">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e7d4b-109">Skapa en MySQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-109">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="e7d4b-110">Ansluta en PHP-app till MySQL</span><span class="sxs-lookup"><span data-stu-id="e7d4b-110">Connect a PHP app to MySQL</span></span>
> * <span data-ttu-id="e7d4b-111">Distribuera appen till Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-111">Deploy the app to Azure</span></span>
> * <span data-ttu-id="e7d4b-112">Uppdatera datamodellen och distribuera appen</span><span class="sxs-lookup"><span data-stu-id="e7d4b-112">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="e7d4b-113">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-113">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="e7d4b-114">Hantera appen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e7d4b-114">Manage the app in the Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e7d4b-115">Krav</span><span class="sxs-lookup"><span data-stu-id="e7d4b-115">Prerequisites</span></span>

<span data-ttu-id="e7d4b-116">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-116">To complete this tutorial:</span></span>

* [<span data-ttu-id="e7d4b-117">Installera Git</span><span class="sxs-lookup"><span data-stu-id="e7d4b-117">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="e7d4b-118">Installera PHP 5.6.4 eller senare</span><span class="sxs-lookup"><span data-stu-id="e7d4b-118">Install PHP 5.6.4 or above</span></span>](http://php.net/downloads.php)
* [<span data-ttu-id="e7d4b-119">Installera Composer</span><span class="sxs-lookup"><span data-stu-id="e7d4b-119">Install Composer</span></span>](https://getcomposer.org/doc/00-intro.md)
* <span data-ttu-id="e7d4b-120">Aktivera följande PHP-tillägg Laravel behov: OpenSSL, PDO MySQL, Mbstring, Tokenizer, XML</span><span class="sxs-lookup"><span data-stu-id="e7d4b-120">Enable the following PHP extensions Laravel needs: OpenSSL, PDO-MySQL, Mbstring, Tokenizer, XML</span></span>
* [<span data-ttu-id="e7d4b-121">Installera och starta MySQL</span><span class="sxs-lookup"><span data-stu-id="e7d4b-121">Install and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a><span data-ttu-id="e7d4b-122">Förbereda lokala MySQL</span><span class="sxs-lookup"><span data-stu-id="e7d4b-122">Prepare local MySQL</span></span>

<span data-ttu-id="e7d4b-123">I det här steget kan skapa du en databas i din lokala MySQL-server för användning i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-123">In this step, you create a database in your local MySQL server for your use in this tutorial.</span></span>

### <a name="connect-to-local-mysql-server"></a><span data-ttu-id="e7d4b-124">Ansluta till lokala MySQL-servern</span><span class="sxs-lookup"><span data-stu-id="e7d4b-124">Connect to local MySQL server</span></span>

<span data-ttu-id="e7d4b-125">Anslut till din lokala MySQL-server i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-125">In a terminal window, connect to your local MySQL server.</span></span> <span data-ttu-id="e7d4b-126">Du kan använda den här terminalfönster för att köra alla kommandon i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-126">You can use this terminal window to run all the commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="e7d4b-127">Om du uppmanas att ange ett lösenord anger du lösenordet för den `root` konto.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-127">If you're prompted for a password, enter the password for the `root` account.</span></span> <span data-ttu-id="e7d4b-128">Om du inte kommer ihåg rotlösenordet, se [MySQL: hur du återställer Rotlösenordet](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-128">If you don't remember your root account password, see [MySQL: How to Reset the Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="e7d4b-129">Om kommandot körs utan problem, är MySQL-servern igång.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-129">If your command runs successfully, then your MySQL server is running.</span></span> <span data-ttu-id="e7d4b-130">Om inte, kontrollera att den lokala MySQL-servern är igång genom att följa den [MySQL efter installationssteg](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-130">If not, make sure that your local MySQL server is started by following the [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database-locally"></a><span data-ttu-id="e7d4b-131">Skapa en databas lokalt</span><span class="sxs-lookup"><span data-stu-id="e7d4b-131">Create a database locally</span></span>

<span data-ttu-id="e7d4b-132">På den `mysql` uppmanar, skapa en databas.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-132">At the `mysql` prompt, create a database.</span></span>

```sql 
CREATE DATABASE sampledb;
```

<span data-ttu-id="e7d4b-133">Avsluta anslutningen till servern genom att skriva `quit`.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-133">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a><span data-ttu-id="e7d4b-134">Skapa en PHP-app lokalt</span><span class="sxs-lookup"><span data-stu-id="e7d4b-134">Create a PHP app locally</span></span>
<span data-ttu-id="e7d4b-135">I det här steget hämta ett Laravel exempelprogram, konfigurera dess databasanslutningen och köra det lokalt.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-135">In this step, you get a Laravel sample application, configure its database connection, and run it locally.</span></span> 

### <a name="clone-the-sample"></a><span data-ttu-id="e7d4b-136">Klona exemplet</span><span class="sxs-lookup"><span data-stu-id="e7d4b-136">Clone the sample</span></span>

<span data-ttu-id="e7d4b-137">I fönstret terminal `cd` till en arbetskatalog.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-137">In the terminal window, `cd` to a working directory.</span></span>

<span data-ttu-id="e7d4b-138">Klona exempellagringsplatsen med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-138">Run the following command to clone the sample repository.</span></span>

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

<span data-ttu-id="e7d4b-139">`cd`till klonade-katalogen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-139">`cd` to your cloned directory.</span></span>
<span data-ttu-id="e7d4b-140">Installera de nödvändiga paketen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-140">Install the required packages.</span></span>

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a><span data-ttu-id="e7d4b-141">Konfigurera MySQL-anslutning</span><span class="sxs-lookup"><span data-stu-id="e7d4b-141">Configure MySQL connection</span></span>

<span data-ttu-id="e7d4b-142">Skapa en fil med namnet i Lagringsplatsens rot *.env*.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-142">In the repository root, create a file named *.env*.</span></span> <span data-ttu-id="e7d4b-143">Kopiera följande variabler i den *.env* fil.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-143">Copy the following variables into the *.env* file.</span></span> <span data-ttu-id="e7d4b-144">Ersätt den  _&lt;root_password >_ med MySQL rotanvändarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-144">Replace the _&lt;root_password>_ placeholder with the MySQL root user's password.</span></span>

```
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

<span data-ttu-id="e7d4b-145">Mer information om hur Laravel använder den _.env_ fil, se [Laravel miljö Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-145">For information on how Laravel uses the _.env_ file, see [Laravel Environment Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration).</span></span>

### <a name="run-the-sample-locally"></a><span data-ttu-id="e7d4b-146">Köra exemplet lokalt</span><span class="sxs-lookup"><span data-stu-id="e7d4b-146">Run the sample locally</span></span>

<span data-ttu-id="e7d4b-147">Kör [Laravel databasen migreringar](https://laravel.com/docs/5.4/migrations) skapa tabellerna programmet behöver.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-147">Run [Laravel database migrations](https://laravel.com/docs/5.4/migrations) to create the tables the application needs.</span></span> <span data-ttu-id="e7d4b-148">Om du vill se vilka tabeller skapas av migreringar finns i den _databasen/migreringar_ katalog i Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-148">To see which tables are created in the migrations, look in the _database/migrations_ directory in the Git repository.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="e7d4b-149">Skapa en ny Laravel programmet nyckel.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-149">Generate a new Laravel application key.</span></span>

```bash
php artisan key:generate
```

<span data-ttu-id="e7d4b-150">Kör appen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-150">Run the application.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="e7d4b-151">Gå till `http://localhost:8000` i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-151">Navigate to `http://localhost:8000` in a browser.</span></span> <span data-ttu-id="e7d4b-152">Lägg till några åtgärder på sidan.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-152">Add a few tasks in the page.</span></span>

![PHP ansluter har till MySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="e7d4b-154">Om du vill stoppa PHP skriver `Ctrl + C` i terminalen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-154">To stop PHP, type `Ctrl + C` in the terminal.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a><span data-ttu-id="e7d4b-155">Skapa MySQL i Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-155">Create MySQL in Azure</span></span>

<span data-ttu-id="e7d4b-156">I det här steget skapar du en MySQL-databas i [Azure-databas för MySQL (förhandsgranskning)](/azure/mysql).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-156">In this step, you create a MySQL database in [Azure Database for MySQL (Preview)](/azure/mysql).</span></span> <span data-ttu-id="e7d4b-157">Senare kan konfigurera du PHP-program att ansluta till den här databasen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-157">Later, you configure the PHP application to connect to this database.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="e7d4b-158">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e7d4b-158">Create a resource group</span></span>

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a><span data-ttu-id="e7d4b-159">Skapa en MySQL-server</span><span class="sxs-lookup"><span data-stu-id="e7d4b-159">Create a MySQL server</span></span>

<span data-ttu-id="e7d4b-160">Skapa en server i Azure-databas för MySQL (förhandsversion) med den [az mysql-servern skapa](/cli/azure/mysql/server#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-160">Create a server in Azure Database for MySQL (Preview) with the [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>

<span data-ttu-id="e7d4b-161">Ersätt namnet på MySQL-servern där du ser i följande kommando i  _&lt;mysql_server_name >_ platshållare (giltiga tecken är `a-z`, `0-9`, och `-`).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-161">In the following command, substitute your MySQL server name where you see the _&lt;mysql_server_name>_ placeholder (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="e7d4b-162">Det här namnet är en del av MySQL-serverns värdnamn (`<mysql_server_name>.database.windows.net`), den måste vara globalt unika.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-162">This name is part of the MySQL server's hostname  (`<mysql_server_name>.database.windows.net`), it needs to be globally unique.</span></span>

```azurecli-interactive
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --admin-user adminuser \
    --admin-password MySQLAzure2017
```

<span data-ttu-id="e7d4b-163">När MySQL-servern har skapats visas Azure CLI information liknar följande exempel:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-163">When the MySQL server is created, the Azure CLI shows information similar to the following example:</span></span>

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a><span data-ttu-id="e7d4b-164">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="e7d4b-164">Configure server firewall</span></span>

<span data-ttu-id="e7d4b-165">Skapa en brandväggsregel för MySQL-servern att tillåta klientanslutningar med hjälp av den [az mysql-brandväggsregel skapa](/cli/azure/mysql/server/firewall-rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-165">Create a firewall rule for your MySQL server to allow client connections by using the [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span>

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="e7d4b-166">Azure-databas för MySQL (förhandsgranskning) begränsa inte för närvarande anslutningar endast till Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-166">Azure Database for MySQL (Preview) doesn't currently limit connections only to Azure services.</span></span> <span data-ttu-id="e7d4b-167">IP-adresser i Azure tilldelas dynamiskt, är det bättre att aktivera alla IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-167">As IP addresses in Azure are dynamically assigned, it is better to enable all IP addresses.</span></span> <span data-ttu-id="e7d4b-168">Tjänsten är i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-168">The service is in preview.</span></span> <span data-ttu-id="e7d4b-169">Bättre metoder för att skydda databasen planeras.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-169">Better methods for securing your database are planned.</span></span>
>
>

### <a name="connect-to-production-mysql-server-locally"></a><span data-ttu-id="e7d4b-170">Ansluta till MySQL produktionsservern lokalt</span><span class="sxs-lookup"><span data-stu-id="e7d4b-170">Connect to production MySQL server locally</span></span>

<span data-ttu-id="e7d4b-171">Anslut till MySQL-server i Azure i fönstret terminal.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-171">In the terminal window, connect to the MySQL server in Azure.</span></span> <span data-ttu-id="e7d4b-172">Använd värdet du angav tidigare för  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-172">Use the value you specified previously for _&lt;mysql_server_name>_.</span></span>

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

<span data-ttu-id="e7d4b-173">När du uppmanas att ange ett lösenord, Använd _tr0ngPa $$ w0rd!_, som du angav när du skapade databasen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-173">When prompted for a password, use _$tr0ngPa$w0rd!_, which you specified when you created the database.</span></span>

### <a name="create-a-production-database"></a><span data-ttu-id="e7d4b-174">Skapa en produktionsdatabas</span><span class="sxs-lookup"><span data-stu-id="e7d4b-174">Create a production database</span></span>

<span data-ttu-id="e7d4b-175">På den `mysql` uppmanar, skapa en databas.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-175">At the `mysql` prompt, create a database.</span></span>

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="e7d4b-176">Skapa en användare med behörighet</span><span class="sxs-lookup"><span data-stu-id="e7d4b-176">Create a user with permissions</span></span>

<span data-ttu-id="e7d4b-177">Skapa en databasanvändare som kallas _phpappuser_ och ge alla behörigheter i den `sampledb` databas.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-177">Create a database user called _phpappuser_ and give it all privileges in the `sampledb` database.</span></span>

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'phpappuser';
```

<span data-ttu-id="e7d4b-178">Avsluta server-anslutningen genom att skriva `quit`.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-178">Exit the server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a><span data-ttu-id="e7d4b-179">Anslut appen till Azure MySQL</span><span class="sxs-lookup"><span data-stu-id="e7d4b-179">Connect app to Azure MySQL</span></span>

<span data-ttu-id="e7d4b-180">I det här steget kan ansluta du PHP-program på MySQL-databas som du skapade i Azure-databas för MySQL (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-180">In this step, you connect the PHP application to the MySQL database you created in Azure Database for MySQL (Preview).</span></span>

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a><span data-ttu-id="e7d4b-181">Konfigurera anslutningen till databasen</span><span class="sxs-lookup"><span data-stu-id="e7d4b-181">Configure the database connection</span></span>

<span data-ttu-id="e7d4b-182">I Lagringsplatsens rot, skapa en _. env.production_ filen och kopiera följande variabler i den.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-182">In the repository root, create an _.env.production_ file and copy the following variables into it.</span></span> <span data-ttu-id="e7d4b-183">Ersätt platshållaren  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-183">Replace the placeholder _&lt;mysql_server_name>_.</span></span>

```
APP_ENV=production
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.database.windows.net
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql_server_name>
DB_PASSWORD=MySQLAzure2017
MYSQL_SSL=true
```

<span data-ttu-id="e7d4b-184">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-184">Save the changes.</span></span>

> [!TIP]
> <span data-ttu-id="e7d4b-185">Om du vill skydda din MySQL anslutningsinformationen den här filen har redan exkluderats från Git-lagringsplats (se _.gitignore_ i Lagringsplatsens rot).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-185">To secure your MySQL connection information, this file is already excluded from the Git repository (See _.gitignore_ in the repository root).</span></span> <span data-ttu-id="e7d4b-186">Senare kan du lära dig hur du konfigurerar miljövariabler i App Service för att ansluta till databasen i Azure-databas för MySQL (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-186">Later, you learn how to configure environment variables in App Service to connect to your database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="e7d4b-187">Med miljövariabler, behöver du inte den *.env* filen i App Service.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-187">With environment variables, you don't need the *.env* file in App Service.</span></span>
>

### <a name="configure-ssl-certificate"></a><span data-ttu-id="e7d4b-188">Konfigurera SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="e7d4b-188">Configure SSL certificate</span></span>

<span data-ttu-id="e7d4b-189">Som standard tillämpar Azure-databas för MySQL SSL-anslutningar från klienter.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-189">By default, Azure Database for MySQL enforces SSL connections from clients.</span></span> <span data-ttu-id="e7d4b-190">Om du vill ansluta till din MySQL-databas i Azure måste du använda en _.pem_ SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-190">To connect to your MySQL database in Azure, you must use a _.pem_ SSL certificate.</span></span>

<span data-ttu-id="e7d4b-191">Öppna _config/database.php_ och lägga till den _sslmode_ och _alternativ_ parametrar till `connections.mysql`som visas i följande kod.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-191">Open _config/database.php_ and add the _sslmode_ and _options_ parameters to `connections.mysql`, as shown in the following code.</span></span>

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

<span data-ttu-id="e7d4b-192">Information om hur du genererar detta _certificate.pem_, se [Konfigurera SSL-anslutning i ditt program för att ansluta säkert till Azure-databas för MySQL](../mysql/howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-192">To learn how to generate this _certificate.pem_, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](../mysql/howto-configure-ssl.md).</span></span>

> [!TIP]
> <span data-ttu-id="e7d4b-193">Sökvägen _/ssl/certificate.pem_ pekar på ett befintligt _certificate.pem_ filen i Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-193">The path _/ssl/certificate.pem_ points to an existing _certificate.pem_ file in the Git repository.</span></span> <span data-ttu-id="e7d4b-194">Den här filen finns i informationssyfte i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-194">This file is provided for convenience in this tutorial.</span></span> <span data-ttu-id="e7d4b-195">För bästa praxis bör du inte utföra din _.pem_ certifikat till källkontroll.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-195">For best practice, you should not commit your _.pem_ certificates into source control.</span></span> 
>

### <a name="test-the-application-locally"></a><span data-ttu-id="e7d4b-196">Testa programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="e7d4b-196">Test the application locally</span></span>

<span data-ttu-id="e7d4b-197">Kör Laravel databasen migreringar med _. env.production_ som filen miljö att skapa tabellerna i din MySQL-databas på Azure-databas för MySQL (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-197">Run Laravel database migrations with _.env.production_ as the environment file to create the tables in your MySQL database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="e7d4b-198">Kom ihåg att _. env.production_ har anslutningsinformationen så att MySQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-198">Remember that _.env.production_ has the connection information to your MySQL database in Azure.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="e7d4b-199">_. env.production_ inte redan har en giltig App-nyckel.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-199">_.env.production_ doesn't have a valid application key yet.</span></span> <span data-ttu-id="e7d4b-200">Generera en ny för det i terminalen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-200">Generate a new one for it in the terminal.</span></span>

```bash
php artisan key:generate --env=production --force
```

<span data-ttu-id="e7d4b-201">Kör exempelprogrammet med _. env.production_ som filen miljö.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-201">Run the sample application with _.env.production_ as the environment file.</span></span>

```bash
php artisan serve --env=production
```

<span data-ttu-id="e7d4b-202">Navigera till `http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-202">Navigate to `http://localhost:8000`.</span></span> <span data-ttu-id="e7d4b-203">Om sidan läses in utan fel, ansluter PHP-program till MySQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-203">If the page loads without errors, the PHP application is connecting to the MySQL database in Azure.</span></span>

<span data-ttu-id="e7d4b-204">Lägg till några åtgärder på sidan.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-204">Add a few tasks in the page.</span></span>

![PHP kan upprätta anslutningen till Azure-databas för MySQL (förhandsgranskning)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="e7d4b-206">Om du vill stoppa PHP skriver `Ctrl + C` i terminalen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-206">To stop PHP, type `Ctrl + C` in the terminal.</span></span>

### <a name="commit-your-changes"></a><span data-ttu-id="e7d4b-207">Genomför ändringarna</span><span class="sxs-lookup"><span data-stu-id="e7d4b-207">Commit your changes</span></span>

<span data-ttu-id="e7d4b-208">Kör följande Git-kommandon för att genomföra ändringarna:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-208">Run the following Git commands to commit your changes:</span></span>

```bash
git add .
git commit -m "database.php updates"
```

<span data-ttu-id="e7d4b-209">Appen är redo att distribueras.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-209">Your app is ready to be deployed.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="e7d4b-210">Distribuera till Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-210">Deploy to Azure</span></span>

<span data-ttu-id="e7d4b-211">I det här steget kan distribuera du MySQL-anslutna PHP-program till Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-211">In this step, you deploy the MySQL-connected PHP application to Azure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="e7d4b-212">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="e7d4b-212">Create an App Service plan</span></span>

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a><span data-ttu-id="e7d4b-213">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="e7d4b-213">Create a web app</span></span>

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)]

### <a name="set-the-php-version"></a><span data-ttu-id="e7d4b-214">Ange PHP-version</span><span class="sxs-lookup"><span data-stu-id="e7d4b-214">Set the PHP version</span></span>

<span data-ttu-id="e7d4b-215">Ange PHP-version som krävs av program med hjälp av den [az webapp konfigurationsuppsättning](/cli/azure/webapp/config#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-215">Set the PHP version that the application requires by using the [az webapp config set](/cli/azure/webapp/config#set) command.</span></span>

<span data-ttu-id="e7d4b-216">Följande kommando anger PHP-version till _7.0_.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-216">The following command sets the PHP version to _7.0_.</span></span>

```azurecli-interactive
az webapp config set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a><span data-ttu-id="e7d4b-217">Konfigurera databasinställningar för</span><span class="sxs-lookup"><span data-stu-id="e7d4b-217">Configure database settings</span></span>

<span data-ttu-id="e7d4b-218">Du kan ansluta till din Azure MySQL-databas med miljövariabler i App Service som pekas tidigare.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-218">As pointed out previously, you can connect to your Azure MySQL database using environment variables in App Service.</span></span>

<span data-ttu-id="e7d4b-219">I App Service som du anger miljövariabler som _appinställningar_ med hjälp av den [az webapp appsettings konfigurationsuppsättning](/cli/azure/webapp/config/appsettings#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-219">In App Service, you set environment variables as _app settings_ by using the [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span>

<span data-ttu-id="e7d4b-220">Följande kommando konfigurerar appinställningarna `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, och `DB_PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-220">The following command configures the app settings `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, and `DB_PASSWORD`.</span></span> <span data-ttu-id="e7d4b-221">Ersätt platshållarna  _&lt;appname >_ och  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-221">Replace the placeholders _&lt;appname>_ and _&lt;mysql_server_name>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

<span data-ttu-id="e7d4b-222">Du kan använda PHP [getenv](http://www.php.net/manual/function.getenv.php) metod för att komma åt inställningarna.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-222">You can use the PHP [getenv](http://www.php.net/manual/function.getenv.php) method to access the settings.</span></span> <span data-ttu-id="e7d4b-223">Laravel koden använder en [env](https://laravel.com/docs/5.4/helpers#method-env) wrapper över PHP `getenv`.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-223">the Laravel code uses an [env](https://laravel.com/docs/5.4/helpers#method-env) wrapper over the PHP `getenv`.</span></span> <span data-ttu-id="e7d4b-224">Till exempel MySQL konfigurationen i _config/database.php_ ser ut som följande kod:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-224">For example, the MySQL configuration in _config/database.php_ looks like the following code:</span></span>

```php
'mysql' => [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    ...
],
```

### <a name="configure-laravel-environment-variables"></a><span data-ttu-id="e7d4b-225">Konfigurera Laravel miljövariabler</span><span class="sxs-lookup"><span data-stu-id="e7d4b-225">Configure Laravel environment variables</span></span>

<span data-ttu-id="e7d4b-226">Laravel måste en tangent i App Service.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-226">Laravel needs an application key in App Service.</span></span> <span data-ttu-id="e7d4b-227">Du kan konfigurera den med app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-227">You can configure it with app settings.</span></span>

<span data-ttu-id="e7d4b-228">Använd `php artisan` att generera en ny nyckel för program utan att spara den till _.env_.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-228">Use `php artisan` to generate a new application key without saving it to _.env_.</span></span>

```bash
php artisan key:generate --show
```

<span data-ttu-id="e7d4b-229">Ange nyckeln för programmet i App Service webbapp med hjälp av den [az webapp konfigurationsuppsättning appsettings](/cli/azure/webapp/config/appsettings#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-229">Set the application key in the App Service web app by using the [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span> <span data-ttu-id="e7d4b-230">Ersätt platshållarna  _&lt;appname >_ och  _&lt;outputofphpartisankey: Generera >_.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-230">Replace the placeholders _&lt;appname>_ and _&lt;outputofphpartisankey:generate>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

<span data-ttu-id="e7d4b-231">`APP_DEBUG="true"`Anger Laravel returnerar felsökningsinformation när den distribuerade webbappen påträffar ett fel.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-231">`APP_DEBUG="true"` tells Laravel to return debugging information when the deployed web app encounters errors.</span></span> <span data-ttu-id="e7d4b-232">När du kör ett produktionsprogram värdet `false`, vilket är säkrare.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-232">When running a production application, set it to `false`, which is more secure.</span></span>

### <a name="set-the-virtual-application-path"></a><span data-ttu-id="e7d4b-233">Ange sökväg till virtuella program</span><span class="sxs-lookup"><span data-stu-id="e7d4b-233">Set the virtual application path</span></span>

<span data-ttu-id="e7d4b-234">Ange den virtuella sökvägen för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-234">Set the virtual application path for the web app.</span></span> <span data-ttu-id="e7d4b-235">Det här steget är nödvändigt eftersom den [Laravel programmet livscykel](https://laravel.com/docs/5.4/lifecycle) börjar i den _offentliga_ katalog i stället för i tillämpningsprogrammets rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-235">This step is required because the [Laravel application lifecycle](https://laravel.com/docs/5.4/lifecycle) begins in the _public_ directory instead of the application's root directory.</span></span> <span data-ttu-id="e7d4b-236">Andra PHP-ramverk vars livscykel startar i rotkatalogen kan arbeta utan manuell konfiguration av den virtuella sökvägen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-236">Other PHP frameworks whose lifecycle start in the root directory can work without manual configuration of the virtual application path.</span></span>

<span data-ttu-id="e7d4b-237">Ange sökvägen till virtuella programmet med hjälp av den [az resurs uppdaterades](/cli/azure/resource#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-237">Set the virtual application path by using the [az resource update](/cli/azure/resource#update) command.</span></span> <span data-ttu-id="e7d4b-238">Ersätt den  _&lt;appname >_ platshållare.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-238">Replace the _&lt;appname>_ placeholder.</span></span>

```azurecli-interactive
az resource update \
    --name web \
    --resource-group myResourceGroup \
    --namespace Microsoft.Web \
    --resource-type config \
    --parent sites/<app_name> \
    --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" \
    --api-version 2015-06-01
```

<span data-ttu-id="e7d4b-239">Som standard pekar rotsökvägen för virtuella program i Azure App Service (_/_) till rotkatalogen för distribuerade programfilerna (_sites\wwwroot_).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-239">By default, Azure App Service points the root virtual application path (_/_) to the root directory of the deployed application files (_sites\wwwroot_).</span></span>

### <a name="configure-a-deployment-user"></a><span data-ttu-id="e7d4b-240">Konfigurera en distributionsanvändare</span><span class="sxs-lookup"><span data-stu-id="e7d4b-240">Configure a deployment user</span></span>

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="configure-local-git-deployment"></a><span data-ttu-id="e7d4b-241">Konfigurera den lokala Git-distributionen</span><span class="sxs-lookup"><span data-stu-id="e7d4b-241">Configure local Git deployment</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git-no-h.md)]

### <a name="push-to-azure-from-git"></a><span data-ttu-id="e7d4b-242">Skicka till Azure från Git</span><span class="sxs-lookup"><span data-stu-id="e7d4b-242">Push to Azure from Git</span></span>

<span data-ttu-id="e7d4b-243">Lägg till en Azure-fjärrdatabas till din lokala Git-databas.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-243">Add an Azure remote to your local Git repository.</span></span>

```bash
git remote add azure <paste_copied_url_here>
```

<span data-ttu-id="e7d4b-244">Skicka till Azure remote distribuera PHP-program.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-244">Push to the Azure remote to deploy the PHP application.</span></span> <span data-ttu-id="e7d4b-245">Du ombeds ange lösenordet du angav tidigare som en del av skapandet av distribution av användaren.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-245">You are prompted for the password you supplied earlier as part of the creation of the deployment user.</span></span>

```bash
git push azure master
```

<span data-ttu-id="e7d4b-246">Under distributionen kommunicerar förloppet med Git i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-246">During deployment, Azure App Service communicates its progress with Git.</span></span>

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

> [!NOTE]
> <span data-ttu-id="e7d4b-247">Det kan hända att distributionsprocessen installerar [Composer](https://getcomposer.org/) paket i slutet.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-247">You may notice that the deployment process installs [Composer](https://getcomposer.org/) packages at the end.</span></span> <span data-ttu-id="e7d4b-248">App Service körs inte dessa automatiseringar under distributionen av standard, så det här exemplet databasen har tre ytterligare filer i rotkatalogen för att den:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-248">App Service does not run these automations during default deployment, so this sample repository has three additional files in its root directory to enable it:</span></span>
>
> - <span data-ttu-id="e7d4b-249">`.deployment`-Den här filen talar om App Service för att köra `bash deploy.sh` som anpassade distributions-skriptet.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-249">`.deployment` - This file tells App Service to run `bash deploy.sh` as the custom deployment script.</span></span>
> - <span data-ttu-id="e7d4b-250">`deploy.sh`-Anpassade skriptet för distribution.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-250">`deploy.sh` - The custom deployment script.</span></span> <span data-ttu-id="e7d4b-251">Om du granska filen ser du att den körs `php composer.phar install` när `npm install`.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-251">If you review the file, you will see that it runs `php composer.phar install` after `npm install`.</span></span>
> - <span data-ttu-id="e7d4b-252">`composer.phar`-Composer package manager.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-252">`composer.phar` - The Composer package manager.</span></span>
>
> <span data-ttu-id="e7d4b-253">Du kan använda den här metoden för att lägga till något steg i distributionen Git-baserade till App Service.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-253">You can use this approach to add any step to your Git-based deployment to App Service.</span></span> <span data-ttu-id="e7d4b-254">Mer information finns i [anpassat distributionsskriptet](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-254">For more information, see [Custom Deployment Script](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span></span>
>

### <a name="browse-to-the-azure-web-app"></a><span data-ttu-id="e7d4b-255">Bläddra till Azure-webbappen</span><span class="sxs-lookup"><span data-stu-id="e7d4b-255">Browse to the Azure web app</span></span>

<span data-ttu-id="e7d4b-256">Bläddra till `http://<app_name>.azurewebsites.net` och lägga till några åtgärder i listan.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-256">Browse to `http://<app_name>.azurewebsites.net` and add a few tasks to the list.</span></span>

![PHP-app som körs i Azure App Service](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

<span data-ttu-id="e7d4b-258">Grattis, du kör en datadrivna PHP-app i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-258">Congratulations, you're running a data-driven PHP app in Azure App Service.</span></span>

## <a name="update-model-locally-and-redeploy"></a><span data-ttu-id="e7d4b-259">Uppdatera modellen lokalt och distribuera</span><span class="sxs-lookup"><span data-stu-id="e7d4b-259">Update model locally and redeploy</span></span>

<span data-ttu-id="e7d4b-260">I det här steget du ändrar enkla till den `task` datamodellen och webapp, och sedan publicera uppdateringen till Azure.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-260">In this step, you make a simple change to the `task` data model and the webapp, and then publish the update to Azure.</span></span>

<span data-ttu-id="e7d4b-261">För scenariot uppgifter ändra programmet så att du kan markera en aktivitet som slutförd.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-261">For the tasks scenario, you modify the application so that you can mark a task as complete.</span></span>

### <a name="add-a-column"></a><span data-ttu-id="e7d4b-262">Lägg till en kolumn</span><span class="sxs-lookup"><span data-stu-id="e7d4b-262">Add a column</span></span>

<span data-ttu-id="e7d4b-263">Navigera till roten för Git-lagringsplats i terminalen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-263">In the terminal, navigate to the root of the Git repository.</span></span>

<span data-ttu-id="e7d4b-264">Generera en ny Databasmigrering för den `tasks` tabellen:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-264">Generate a new database migration for the `tasks` table:</span></span>

```bash
php artisan make:migration add_complete_column --table=tasks
```

<span data-ttu-id="e7d4b-265">Det här kommandot visar namnet på migreringsfilen som genereras.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-265">This command shows you the name of the migration file that's generated.</span></span> <span data-ttu-id="e7d4b-266">Den här filen i _databasen/migreringar_ och öppna den.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-266">Find this file in _database/migrations_ and open it.</span></span>

<span data-ttu-id="e7d4b-267">Ersätt den `up` metoden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-267">Replace the `up` method with the following code:</span></span>

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

<span data-ttu-id="e7d4b-268">Föregående kod lägger till en boolesk kolumn i den `tasks` tabell som kallas `complete`.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-268">The preceding code adds a boolean column in the `tasks` table called `complete`.</span></span>

<span data-ttu-id="e7d4b-269">Ersätt den `down` metod med följande kod för Återföringsåtgärd:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-269">Replace the `down` method with the following code for the rollback action:</span></span>

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

<span data-ttu-id="e7d4b-270">I terminalen och kör Laravel databasen migrering för att göra ändringen i den lokala databasen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-270">In the terminal, run Laravel database migrations to make the change in the local database.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="e7d4b-271">Baserat på den [Laravel namngivningskonvention](https://laravel.com/docs/5.4/eloquent#defining-models), modellen `Task` (se _app/Task.php_) mappas till den `tasks` tabellen som standard.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-271">Based on the [Laravel naming convention](https://laravel.com/docs/5.4/eloquent#defining-models), the model `Task` (see _app/Task.php_) maps to the `tasks` table by default.</span></span>

### <a name="update-application-logic"></a><span data-ttu-id="e7d4b-272">Uppdatera programlogik</span><span class="sxs-lookup"><span data-stu-id="e7d4b-272">Update application logic</span></span>

<span data-ttu-id="e7d4b-273">Öppna den *routes/web.php* fil.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-273">Open the *routes/web.php* file.</span></span> <span data-ttu-id="e7d4b-274">Programmet definierar dess vägar och affärslogik här.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-274">The application defines its routes and business logic here.</span></span>

<span data-ttu-id="e7d4b-275">Lägga till en väg med följande kod i slutet av filen:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-275">At the end of the file, add a route with the following code:</span></span>

```php
/**
 * Toggle Task completeness
 */
Route::post('/task/{id}', function ($id) {
    error_log('INFO: post /task/'.$id);
    $task = Task::findOrFail($id);

    $task->complete = !$task->complete;
    $task->save();

    return redirect('/');
});
```

<span data-ttu-id="e7d4b-276">Föregående kod gör en enkel uppdatering till datamodellen genom att klicka på värdet för `complete`.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-276">The preceding code makes a simple update to the data model by toggling the value of `complete`.</span></span>

### <a name="update-the-view"></a><span data-ttu-id="e7d4b-277">Uppdatera vyn</span><span class="sxs-lookup"><span data-stu-id="e7d4b-277">Update the view</span></span>

<span data-ttu-id="e7d4b-278">Öppna den *resources/views/tasks.blade.php* fil.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-278">Open the *resources/views/tasks.blade.php* file.</span></span> <span data-ttu-id="e7d4b-279">Hitta de `<tr>` startkoden och Ersätt den med:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-279">Find the `<tr>` opening tag and replace it with:</span></span>

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

<span data-ttu-id="e7d4b-280">Föregående kod ändrar färg raden beroende på om aktiviteten är klar.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-280">The preceding code changes the row color depending on whether the task is complete.</span></span>

<span data-ttu-id="e7d4b-281">På nästa rad kan du använda följande kod:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-281">In the next line, you have the following code:</span></span>

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

<span data-ttu-id="e7d4b-282">Ersätt hela raden med följande kod:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-282">Replace the entire line with the following code:</span></span>

```html
<td>
    <form action="{{ url('task/'.$task->id) }}" method="POST">
        {{ csrf_field() }}

        <button type="submit" class="btn btn-xs">
            <i class="fa {{$task->complete ? 'fa-check-square-o' : 'fa-square-o'}}"></i>
        </button>
        {{ $task->name }}
    </form>
</td>
```

<span data-ttu-id="e7d4b-283">Föregående kod lägger till skickaknappen som refererar till det flöde som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-283">The preceding code adds the submit button that references the route that you defined earlier.</span></span>

### <a name="test-the-changes-locally"></a><span data-ttu-id="e7d4b-284">Testa ändringar lokalt</span><span class="sxs-lookup"><span data-stu-id="e7d4b-284">Test the changes locally</span></span>

<span data-ttu-id="e7d4b-285">Kör utvecklingsservern från rotkatalogen på Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-285">From the root directory of the Git repository, run the development server.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="e7d4b-286">Att se aktivitetsstatus ändra, gå till `http://localhost:8000` och markera kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-286">To see the task status change, navigate to `http://localhost:8000` and select the checkbox.</span></span>

![Tillagda kryssrutan till aktivitet](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

<span data-ttu-id="e7d4b-288">Om du vill stoppa PHP skriver `Ctrl + C` i terminalen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-288">To stop PHP, type `Ctrl + C` in the terminal.</span></span>

### <a name="publish-changes-to-azure"></a><span data-ttu-id="e7d4b-289">Publicera ändringar i Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-289">Publish changes to Azure</span></span>

<span data-ttu-id="e7d4b-290">I terminalen och kör Laravel databasen migreringar med anslutningssträngen produktion göra ändringar i Azure-databasen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-290">In the terminal, run Laravel database migrations with the production connection string to make the change in the Azure database.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="e7d4b-291">Genomför alla ändringar i Git och skicka sedan koden ändringarna till Azure.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-291">Commit all the changes in Git, and then push the code changes to Azure.</span></span>

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

<span data-ttu-id="e7d4b-292">En gång i `git push` är klar, gå till Azure webbapp och testa de nya funktionerna.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-292">Once the `git push` is complete, navigate to the Azure web app and test the new functionality.</span></span>

![Ändringar i modellen och databasen publiceras på Azure](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="e7d4b-294">Om du har lagt till alla uppgifter som finns kvar i databasen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-294">If you added any tasks, they are retained in the database.</span></span> <span data-ttu-id="e7d4b-295">Uppdateringar till dataschemat lämna befintliga data intakta.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-295">Updates to the data schema leave existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="e7d4b-296">Dataströmmen diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="e7d4b-296">Stream diagnostic logs</span></span>

<span data-ttu-id="e7d4b-297">Du kan hämta loggarna för konsolen skickas till terminalen när PHP-program körs i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-297">While the PHP application runs in Azure App Service, you can get the console logs piped to your terminal.</span></span> <span data-ttu-id="e7d4b-298">På så sätt kan du få samma diagnostiska meddelanden för att felsöka programfel.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-298">That way, you can get the same diagnostic messages to help you debug application errors.</span></span>

<span data-ttu-id="e7d4b-299">Starta loggen strömning med den [az webapp loggen pilslut](/cli/azure/webapp/log#tail) kommando.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-299">To start log streaming, use the [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup
```

<span data-ttu-id="e7d4b-300">Uppdatera Azure-webbapp i webbläsaren att hämta vissa webbtrafik när loggen streaming har startats.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-300">Once log streaming has started, refresh the Azure web app in the browser to get some web traffic.</span></span> <span data-ttu-id="e7d4b-301">Du kan nu se loggarna för konsolen skickas till terminalen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-301">You can now see console logs piped to the terminal.</span></span> <span data-ttu-id="e7d4b-302">Om du inte ser loggarna för konsolen omedelbart, Kontrollera igen i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-302">If you don't see console logs immediately, check again in 30 seconds.</span></span>

<span data-ttu-id="e7d4b-303">Om du vill sluta logga strömning på när som helst, Skriv `Ctrl` + `C`.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-303">To stop log streaming at anytime, type `Ctrl`+`C`.</span></span>

> [!TIP]
> <span data-ttu-id="e7d4b-304">Ett PHP-program kan använda standarden [error_log()](http://php.net/manual/function.error-log.php) till utdata till konsolen.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-304">A PHP application can use the standard [error_log()](http://php.net/manual/function.error-log.php) to output to the console.</span></span> <span data-ttu-id="e7d4b-305">Exempelprogrammet använder den här metoden i _app/Http/routes.php_.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-305">The sample application uses this approach in _app/Http/routes.php_.</span></span>
>
> <span data-ttu-id="e7d4b-306">Som ett webbramverk [Laravel använder Monolog](https://laravel.com/docs/5.4/errors) som loggning-provider.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-306">As a web framework, [Laravel uses Monolog](https://laravel.com/docs/5.4/errors) as the logging provider.</span></span> <span data-ttu-id="e7d4b-307">Information om hur du hämtar Monolog utgående meddelanden till konsolen finns [PHP: hur du använder monolog loggar konsolen (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span><span class="sxs-lookup"><span data-stu-id="e7d4b-307">To see how to get Monolog to output messages to the console, see [PHP: How to use monolog to log to console (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span></span>
>
>

## <a name="manage-the-azure-web-app"></a><span data-ttu-id="e7d4b-308">Hantera Azure-webbappen</span><span class="sxs-lookup"><span data-stu-id="e7d4b-308">Manage the Azure web app</span></span>

<span data-ttu-id="e7d4b-309">Gå till [Azure Portal](https://portal.azure.com) för att hantera den webbapp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-309">Go to the [Azure portal](https://portal.azure.com) to manage the web app you created.</span></span>

<span data-ttu-id="e7d4b-310">Klicka på **Apptjänster** i menyn till vänster och sedan på namnet på din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-310">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigera till webbappen på Azure Portal](./media/app-service-web-tutorial-php-mysql/access-portal.png)

<span data-ttu-id="e7d4b-312">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-312">You see your web app's Overview page.</span></span> <span data-ttu-id="e7d4b-313">Här kan du utföra grundläggande hanteringsuppgifter som att stoppa, starta, omstart, bläddra och ta bort.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-313">Here, you can perform basic management tasks like  stop, start, restart, browse, and delete.</span></span>

<span data-ttu-id="e7d4b-314">Den vänstra menyn innehåller sidor för att konfigurera din app.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-314">The left menu provides pages for configuring your app.</span></span>

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="e7d4b-316">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e7d4b-316">Next steps</span></span>

<span data-ttu-id="e7d4b-317">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="e7d4b-317">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e7d4b-318">Skapa en MySQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-318">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="e7d4b-319">Ansluta en PHP-app till MySQL</span><span class="sxs-lookup"><span data-stu-id="e7d4b-319">Connect a PHP app to MySQL</span></span>
> * <span data-ttu-id="e7d4b-320">Distribuera appen till Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-320">Deploy the app to Azure</span></span>
> * <span data-ttu-id="e7d4b-321">Uppdatera datamodellen och distribuera appen</span><span class="sxs-lookup"><span data-stu-id="e7d4b-321">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="e7d4b-322">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="e7d4b-322">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="e7d4b-323">Hantera appen i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e7d4b-323">Manage the app in the Azure portal</span></span>

<span data-ttu-id="e7d4b-324">Gå vidare till nästa kurs att lära dig hur du mappar en anpassad DNS-namn till ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="e7d4b-324">Advance to the next tutorial to learn how to map a custom DNS name to a web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e7d4b-325">Mappa ett befintligt anpassat DNS-namn till Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="e7d4b-325">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
