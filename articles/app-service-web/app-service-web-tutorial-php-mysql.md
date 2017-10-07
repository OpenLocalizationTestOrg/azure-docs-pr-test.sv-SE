---
title: aaaBuild en PHP- och MySQL-webbapp i Azure | Microsoft Docs
description: "Lär dig hur tooget en PHP-app som arbetar i Azure, med anslutning tooa MySQL-databas i Azure."
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
ms.openlocfilehash: 3c050b30e2e2c80d011bed989cd5f8cecac35d15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a><span data-ttu-id="17ba3-103">Skapa en PHP- och MySQL-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="17ba3-103">Build a PHP and MySQL web app in Azure</span></span>

<span data-ttu-id="17ba3-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="17ba3-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="17ba3-105">Den här kursen visar hur toocreate en PHP webbapp i Azure och koppla den tooa MySQL-databas.</span><span class="sxs-lookup"><span data-stu-id="17ba3-105">This tutorial shows how toocreate a PHP web app in Azure and connect it tooa MySQL database.</span></span> <span data-ttu-id="17ba3-106">När du är klar har du en [Laravel](https://laravel.com/) app som körs på Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="17ba3-106">When you're finished, you'll have a [Laravel](https://laravel.com/) app running on Azure App Service Web Apps.</span></span>

![PHP-app som körs i Azure App Service](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="17ba3-108">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="17ba3-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="17ba3-109">Skapa en MySQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="17ba3-109">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="17ba3-110">Ansluta en app tooMySQL för PHP</span><span class="sxs-lookup"><span data-stu-id="17ba3-110">Connect a PHP app tooMySQL</span></span>
> * <span data-ttu-id="17ba3-111">Distribuera hello app tooAzure</span><span class="sxs-lookup"><span data-stu-id="17ba3-111">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="17ba3-112">Uppdatera hello-datamodell och omdistribuera hello app</span><span class="sxs-lookup"><span data-stu-id="17ba3-112">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="17ba3-113">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="17ba3-113">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="17ba3-114">Hantera hello appen i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="17ba3-114">Manage hello app in hello Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17ba3-115">Krav</span><span class="sxs-lookup"><span data-stu-id="17ba3-115">Prerequisites</span></span>

<span data-ttu-id="17ba3-116">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="17ba3-116">toocomplete this tutorial:</span></span>

* [<span data-ttu-id="17ba3-117">Installera Git</span><span class="sxs-lookup"><span data-stu-id="17ba3-117">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="17ba3-118">Installera PHP 5.6.4 eller senare</span><span class="sxs-lookup"><span data-stu-id="17ba3-118">Install PHP 5.6.4 or above</span></span>](http://php.net/downloads.php)
* [<span data-ttu-id="17ba3-119">Installera Composer</span><span class="sxs-lookup"><span data-stu-id="17ba3-119">Install Composer</span></span>](https://getcomposer.org/doc/00-intro.md)
* <span data-ttu-id="17ba3-120">Aktivera följande PHP-tillägg Laravel behov hello: OpenSSL, PDO MySQL, Mbstring, Tokenizer, XML</span><span class="sxs-lookup"><span data-stu-id="17ba3-120">Enable hello following PHP extensions Laravel needs: OpenSSL, PDO-MySQL, Mbstring, Tokenizer, XML</span></span>
* [<span data-ttu-id="17ba3-121">Installera och starta MySQL</span><span class="sxs-lookup"><span data-stu-id="17ba3-121">Install and start MySQL</span></span>](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a><span data-ttu-id="17ba3-122">Förbereda lokala MySQL</span><span class="sxs-lookup"><span data-stu-id="17ba3-122">Prepare local MySQL</span></span>

<span data-ttu-id="17ba3-123">I det här steget kan skapa du en databas i din lokala MySQL-server för användning i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="17ba3-123">In this step, you create a database in your local MySQL server for your use in this tutorial.</span></span>

### <a name="connect-toolocal-mysql-server"></a><span data-ttu-id="17ba3-124">Anslut toolocal MySQL-server</span><span class="sxs-lookup"><span data-stu-id="17ba3-124">Connect toolocal MySQL server</span></span>

<span data-ttu-id="17ba3-125">Anslut tooyour lokala MySQL-servern i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="17ba3-125">In a terminal window, connect tooyour local MySQL server.</span></span> <span data-ttu-id="17ba3-126">Du kan använda den här terminalfönster toorun alla hello-kommandon i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="17ba3-126">You can use this terminal window toorun all hello commands in this tutorial.</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="17ba3-127">Om du uppmanas att ange ett lösenord anger hello lösenordet för hello `root` konto.</span><span class="sxs-lookup"><span data-stu-id="17ba3-127">If you're prompted for a password, enter hello password for hello `root` account.</span></span> <span data-ttu-id="17ba3-128">Om du inte kommer ihåg rotlösenordet, se [MySQL: hur tooReset hello Rotlösenordet](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span><span class="sxs-lookup"><span data-stu-id="17ba3-128">If you don't remember your root account password, see [MySQL: How tooReset hello Root Password](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).</span></span>

<span data-ttu-id="17ba3-129">Om kommandot körs utan problem, är MySQL-servern igång.</span><span class="sxs-lookup"><span data-stu-id="17ba3-129">If your command runs successfully, then your MySQL server is running.</span></span> <span data-ttu-id="17ba3-130">Om inte, kontrollera att den lokala MySQL-servern har startats med följande hello [MySQL efter installationssteg](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span><span class="sxs-lookup"><span data-stu-id="17ba3-130">If not, make sure that your local MySQL server is started by following hello [MySQL post-installation steps](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).</span></span>

### <a name="create-a-database-locally"></a><span data-ttu-id="17ba3-131">Skapa en databas lokalt</span><span class="sxs-lookup"><span data-stu-id="17ba3-131">Create a database locally</span></span>

<span data-ttu-id="17ba3-132">Vid hello `mysql` uppmanar, skapa en databas.</span><span class="sxs-lookup"><span data-stu-id="17ba3-132">At hello `mysql` prompt, create a database.</span></span>

```sql 
CREATE DATABASE sampledb;
```

<span data-ttu-id="17ba3-133">Avsluta anslutningen till servern genom att skriva `quit`.</span><span class="sxs-lookup"><span data-stu-id="17ba3-133">Exit your server connection by typing `quit`.</span></span>

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a><span data-ttu-id="17ba3-134">Skapa en PHP-app lokalt</span><span class="sxs-lookup"><span data-stu-id="17ba3-134">Create a PHP app locally</span></span>
<span data-ttu-id="17ba3-135">I det här steget hämta ett Laravel exempelprogram, konfigurera dess databasanslutningen och köra det lokalt.</span><span class="sxs-lookup"><span data-stu-id="17ba3-135">In this step, you get a Laravel sample application, configure its database connection, and run it locally.</span></span> 

### <a name="clone-hello-sample"></a><span data-ttu-id="17ba3-136">Klona hello-exempel</span><span class="sxs-lookup"><span data-stu-id="17ba3-136">Clone hello sample</span></span>

<span data-ttu-id="17ba3-137">I hello terminalfönster, `cd` tooa arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="17ba3-137">In hello terminal window, `cd` tooa working directory.</span></span>

<span data-ttu-id="17ba3-138">Hello kör följande kommando tooclone hello exempel lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="17ba3-138">Run hello following command tooclone hello sample repository.</span></span>

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

<span data-ttu-id="17ba3-139">`cd`tooyour klonade katalog.</span><span class="sxs-lookup"><span data-stu-id="17ba3-139">`cd` tooyour cloned directory.</span></span>
<span data-ttu-id="17ba3-140">Installera hello krävs paket.</span><span class="sxs-lookup"><span data-stu-id="17ba3-140">Install hello required packages.</span></span>

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a><span data-ttu-id="17ba3-141">Konfigurera MySQL-anslutning</span><span class="sxs-lookup"><span data-stu-id="17ba3-141">Configure MySQL connection</span></span>

<span data-ttu-id="17ba3-142">Skapa en fil med namnet i hello Lagringsplatsens rot *.env*.</span><span class="sxs-lookup"><span data-stu-id="17ba3-142">In hello repository root, create a file named *.env*.</span></span> <span data-ttu-id="17ba3-143">Kopiera hello följande variabler i hello *.env* fil.</span><span class="sxs-lookup"><span data-stu-id="17ba3-143">Copy hello following variables into hello *.env* file.</span></span> <span data-ttu-id="17ba3-144">Ersätt hello  _&lt;root_password >_ med hello MySQL rotanvändarens lösenord.</span><span class="sxs-lookup"><span data-stu-id="17ba3-144">Replace hello _&lt;root_password>_ placeholder with hello MySQL root user's password.</span></span>

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

<span data-ttu-id="17ba3-145">Mer information om hur Laravel använder hello _.env_ fil, se [Laravel miljö Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration).</span><span class="sxs-lookup"><span data-stu-id="17ba3-145">For information on how Laravel uses hello _.env_ file, see [Laravel Environment Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration).</span></span>

### <a name="run-hello-sample-locally"></a><span data-ttu-id="17ba3-146">Kör hello exempel lokalt</span><span class="sxs-lookup"><span data-stu-id="17ba3-146">Run hello sample locally</span></span>

<span data-ttu-id="17ba3-147">Kör [Laravel databasen migreringar](https://laravel.com/docs/5.4/migrations) toocreate hello tabeller hello programbehov.</span><span class="sxs-lookup"><span data-stu-id="17ba3-147">Run [Laravel database migrations](https://laravel.com/docs/5.4/migrations) toocreate hello tables hello application needs.</span></span> <span data-ttu-id="17ba3-148">vilka tabeller skapas hello migreringar titta i hello toosee _databasen/migreringar_ katalogen i hello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="17ba3-148">toosee which tables are created in hello migrations, look in hello _database/migrations_ directory in hello Git repository.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="17ba3-149">Skapa en ny Laravel programmet nyckel.</span><span class="sxs-lookup"><span data-stu-id="17ba3-149">Generate a new Laravel application key.</span></span>

```bash
php artisan key:generate
```

<span data-ttu-id="17ba3-150">Kör hello program.</span><span class="sxs-lookup"><span data-stu-id="17ba3-150">Run hello application.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="17ba3-151">Navigera för`http://localhost:8000` i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="17ba3-151">Navigate too`http://localhost:8000` in a browser.</span></span> <span data-ttu-id="17ba3-152">Lägga till några åtgärder på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="17ba3-152">Add a few tasks in hello page.</span></span>

![PHP ansluter har tooMySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="17ba3-154">Skriv toostop PHP `Ctrl + C` i hello terminal.</span><span class="sxs-lookup"><span data-stu-id="17ba3-154">toostop PHP, type `Ctrl + C` in hello terminal.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a><span data-ttu-id="17ba3-155">Skapa MySQL i Azure</span><span class="sxs-lookup"><span data-stu-id="17ba3-155">Create MySQL in Azure</span></span>

<span data-ttu-id="17ba3-156">I det här steget skapar du en MySQL-databas i [Azure-databas för MySQL (förhandsgranskning)](/azure/mysql).</span><span class="sxs-lookup"><span data-stu-id="17ba3-156">In this step, you create a MySQL database in [Azure Database for MySQL (Preview)](/azure/mysql).</span></span> <span data-ttu-id="17ba3-157">Senare kan konfigurera du hello PHP tooconnect toothis databas.</span><span class="sxs-lookup"><span data-stu-id="17ba3-157">Later, you configure hello PHP application tooconnect toothis database.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="17ba3-158">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="17ba3-158">Create a resource group</span></span>

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a><span data-ttu-id="17ba3-159">Skapa en MySQL-server</span><span class="sxs-lookup"><span data-stu-id="17ba3-159">Create a MySQL server</span></span>

<span data-ttu-id="17ba3-160">Skapa en server i Azure-databas för MySQL (förhandsversion) med hello [az mysql-servern skapa](/cli/azure/mysql/server#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="17ba3-160">Create a server in Azure Database for MySQL (Preview) with hello [az mysql server create](/cli/azure/mysql/server#create) command.</span></span>

<span data-ttu-id="17ba3-161">Hello följande kommando, Ersätt namnet på MySQL-servern där du ser hello  _&lt;mysql_server_name >_ platshållare (giltiga tecken är `a-z`, `0-9`, och `-`).</span><span class="sxs-lookup"><span data-stu-id="17ba3-161">In hello following command, substitute your MySQL server name where you see hello _&lt;mysql_server_name>_ placeholder (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="17ba3-162">Det här namnet är en del av hello MySQL serverns värdnamn (`<mysql_server_name>.database.windows.net`), den måste toobe globalt unika.</span><span class="sxs-lookup"><span data-stu-id="17ba3-162">This name is part of hello MySQL server's hostname  (`<mysql_server_name>.database.windows.net`), it needs toobe globally unique.</span></span>

```azurecli-interactive
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --admin-user adminuser \
    --admin-password MySQLAzure2017
```

<span data-ttu-id="17ba3-163">När hello MySQL server skapas visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="17ba3-163">When hello MySQL server is created, hello Azure CLI shows information similar toohello following example:</span></span>

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

### <a name="configure-server-firewall"></a><span data-ttu-id="17ba3-164">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="17ba3-164">Configure server firewall</span></span>

<span data-ttu-id="17ba3-165">Skapa en brandväggsregel för MySQL server tooallow klienten anslutningar med hello [az mysql-brandväggsregel skapa](/cli/azure/mysql/server/firewall-rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="17ba3-165">Create a firewall rule for your MySQL server tooallow client connections by using hello [az mysql server firewall-rule create](/cli/azure/mysql/server/firewall-rule#create) command.</span></span>

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="17ba3-166">Azure-databas för MySQL (förhandsgranskning) begränsa inte för närvarande anslutningar endast tooAzure tjänster.</span><span class="sxs-lookup"><span data-stu-id="17ba3-166">Azure Database for MySQL (Preview) doesn't currently limit connections only tooAzure services.</span></span> <span data-ttu-id="17ba3-167">IP-adresser i Azure tilldelas dynamiskt, är det bättre tooenable alla IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="17ba3-167">As IP addresses in Azure are dynamically assigned, it is better tooenable all IP addresses.</span></span> <span data-ttu-id="17ba3-168">hello-tjänsten är i förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="17ba3-168">hello service is in preview.</span></span> <span data-ttu-id="17ba3-169">Bättre metoder för att skydda databasen planeras.</span><span class="sxs-lookup"><span data-stu-id="17ba3-169">Better methods for securing your database are planned.</span></span>
>
>

### <a name="connect-tooproduction-mysql-server-locally"></a><span data-ttu-id="17ba3-170">Ansluta tooproduction MySQL server lokalt</span><span class="sxs-lookup"><span data-stu-id="17ba3-170">Connect tooproduction MySQL server locally</span></span>

<span data-ttu-id="17ba3-171">Hello terminalfönster, Anslut toohello MySQL-server i Azure.</span><span class="sxs-lookup"><span data-stu-id="17ba3-171">In hello terminal window, connect toohello MySQL server in Azure.</span></span> <span data-ttu-id="17ba3-172">Använd hello-värde som du angav tidigare för  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="17ba3-172">Use hello value you specified previously for _&lt;mysql_server_name>_.</span></span>

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

<span data-ttu-id="17ba3-173">När du uppmanas att ange ett lösenord, Använd _tr0ngPa $$ w0rd!_, som du angav när du skapade hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="17ba3-173">When prompted for a password, use _$tr0ngPa$w0rd!_, which you specified when you created hello database.</span></span>

### <a name="create-a-production-database"></a><span data-ttu-id="17ba3-174">Skapa en produktionsdatabas</span><span class="sxs-lookup"><span data-stu-id="17ba3-174">Create a production database</span></span>

<span data-ttu-id="17ba3-175">Vid hello `mysql` uppmanar, skapa en databas.</span><span class="sxs-lookup"><span data-stu-id="17ba3-175">At hello `mysql` prompt, create a database.</span></span>

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a><span data-ttu-id="17ba3-176">Skapa en användare med behörighet</span><span class="sxs-lookup"><span data-stu-id="17ba3-176">Create a user with permissions</span></span>

<span data-ttu-id="17ba3-177">Skapa en databasanvändare som kallas _phpappuser_ och ge den alla behörigheter i hello `sampledb` databas.</span><span class="sxs-lookup"><span data-stu-id="17ba3-177">Create a database user called _phpappuser_ and give it all privileges in hello `sampledb` database.</span></span>

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* too'phpappuser';
```

<span data-ttu-id="17ba3-178">Avsluta hello serveranslutning genom att skriva `quit`.</span><span class="sxs-lookup"><span data-stu-id="17ba3-178">Exit hello server connection by typing `quit`.</span></span>

```sql
quit
```

## <a name="connect-app-tooazure-mysql"></a><span data-ttu-id="17ba3-179">Ansluta appen tooAzure MySQL</span><span class="sxs-lookup"><span data-stu-id="17ba3-179">Connect app tooAzure MySQL</span></span>

<span data-ttu-id="17ba3-180">I det här steget kan ansluta du hello PHP programmet toohello MySQL-databas du skapade i Azure-databas för MySQL (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="17ba3-180">In this step, you connect hello PHP application toohello MySQL database you created in Azure Database for MySQL (Preview).</span></span>

<a name="devconfig"></a>

### <a name="configure-hello-database-connection"></a><span data-ttu-id="17ba3-181">Konfigurera hello databasanslutning</span><span class="sxs-lookup"><span data-stu-id="17ba3-181">Configure hello database connection</span></span>

<span data-ttu-id="17ba3-182">I Lagringsplatsens rot hello, skapar en _. env.production_ filen och kopiera hello följande variabler i den.</span><span class="sxs-lookup"><span data-stu-id="17ba3-182">In hello repository root, create an _.env.production_ file and copy hello following variables into it.</span></span> <span data-ttu-id="17ba3-183">Ersätt platshållaren hello  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="17ba3-183">Replace hello placeholder _&lt;mysql_server_name>_.</span></span>

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

<span data-ttu-id="17ba3-184">Spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="17ba3-184">Save hello changes.</span></span>

> [!TIP]
> <span data-ttu-id="17ba3-185">toosecure MySQL anslutningsinformationen, den här filen har redan exkluderats från hello Git-lagringsplats (se _.gitignore_ i hello Lagringsplatsens rot).</span><span class="sxs-lookup"><span data-stu-id="17ba3-185">toosecure your MySQL connection information, this file is already excluded from hello Git repository (See _.gitignore_ in hello repository root).</span></span> <span data-ttu-id="17ba3-186">Senare kan du lära dig hur tooconfigure miljövariabler i Apptjänst tooconnect tooyour databasen i Azure-databas för MySQL (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="17ba3-186">Later, you learn how tooconfigure environment variables in App Service tooconnect tooyour database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="17ba3-187">Med miljövariabler, behöver du inte hello *.env* filen i App Service.</span><span class="sxs-lookup"><span data-stu-id="17ba3-187">With environment variables, you don't need hello *.env* file in App Service.</span></span>
>

### <a name="configure-ssl-certificate"></a><span data-ttu-id="17ba3-188">Konfigurera SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="17ba3-188">Configure SSL certificate</span></span>

<span data-ttu-id="17ba3-189">Som standard tillämpar Azure-databas för MySQL SSL-anslutningar från klienter.</span><span class="sxs-lookup"><span data-stu-id="17ba3-189">By default, Azure Database for MySQL enforces SSL connections from clients.</span></span> <span data-ttu-id="17ba3-190">tooconnect tooyour MySQL-databas i Azure, måste du använda en _.pem_ SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="17ba3-190">tooconnect tooyour MySQL database in Azure, you must use a _.pem_ SSL certificate.</span></span>

<span data-ttu-id="17ba3-191">Öppna _config/database.php_ och Lägg till hello _sslmode_ och _alternativ_ parametrar för`connections.mysql`som visas i följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="17ba3-191">Open _config/database.php_ and add hello _sslmode_ and _options_ parameters too`connections.mysql`, as shown in hello following code.</span></span>

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

<span data-ttu-id="17ba3-192">toolearn hur toogenerate detta _certificate.pem_, se [Konfigurera SSL-anslutning i ditt program toosecurely ansluta tooAzure databas för MySQL](../mysql/howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="17ba3-192">toolearn how toogenerate this _certificate.pem_, see [Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL](../mysql/howto-configure-ssl.md).</span></span>

> [!TIP]
> <span data-ttu-id="17ba3-193">hello sökvägen _/ssl/certificate.pem_ pekar tooan befintliga _certificate.pem_ filen i hello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="17ba3-193">hello path _/ssl/certificate.pem_ points tooan existing _certificate.pem_ file in hello Git repository.</span></span> <span data-ttu-id="17ba3-194">Den här filen finns i informationssyfte i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="17ba3-194">This file is provided for convenience in this tutorial.</span></span> <span data-ttu-id="17ba3-195">För bästa praxis bör du inte utföra din _.pem_ certifikat till källkontroll.</span><span class="sxs-lookup"><span data-stu-id="17ba3-195">For best practice, you should not commit your _.pem_ certificates into source control.</span></span> 
>

### <a name="test-hello-application-locally"></a><span data-ttu-id="17ba3-196">Testa hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="17ba3-196">Test hello application locally</span></span>

<span data-ttu-id="17ba3-197">Kör Laravel databasen migreringar med _. env.production_ som hello miljö filen toocreate hello tabeller i MySQL-databas i Azure-databas för MySQL (förhandsversion).</span><span class="sxs-lookup"><span data-stu-id="17ba3-197">Run Laravel database migrations with _.env.production_ as hello environment file toocreate hello tables in your MySQL database in Azure Database for MySQL (Preview).</span></span> <span data-ttu-id="17ba3-198">Kom ihåg att _. env.production_ har hello anslutning information tooyour MySQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="17ba3-198">Remember that _.env.production_ has hello connection information tooyour MySQL database in Azure.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="17ba3-199">_. env.production_ inte redan har en giltig App-nyckel.</span><span class="sxs-lookup"><span data-stu-id="17ba3-199">_.env.production_ doesn't have a valid application key yet.</span></span> <span data-ttu-id="17ba3-200">Generera en ny för det i hello terminal.</span><span class="sxs-lookup"><span data-stu-id="17ba3-200">Generate a new one for it in hello terminal.</span></span>

```bash
php artisan key:generate --env=production --force
```

<span data-ttu-id="17ba3-201">Köra hello exempelprogrammet med _. env.production_ som hello miljö fil.</span><span class="sxs-lookup"><span data-stu-id="17ba3-201">Run hello sample application with _.env.production_ as hello environment file.</span></span>

```bash
php artisan serve --env=production
```

<span data-ttu-id="17ba3-202">Navigera för`http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="17ba3-202">Navigate too`http://localhost:8000`.</span></span> <span data-ttu-id="17ba3-203">Om hello sidan läses in utan fel ansluter hello PHP-program toohello MySQL-databas i Azure.</span><span class="sxs-lookup"><span data-stu-id="17ba3-203">If hello page loads without errors, hello PHP application is connecting toohello MySQL database in Azure.</span></span>

<span data-ttu-id="17ba3-204">Lägga till några åtgärder på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="17ba3-204">Add a few tasks in hello page.</span></span>

![PHP ansluter har tooAzure databas för MySQL (förhandsgranskning)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

<span data-ttu-id="17ba3-206">Skriv toostop PHP `Ctrl + C` i hello terminal.</span><span class="sxs-lookup"><span data-stu-id="17ba3-206">toostop PHP, type `Ctrl + C` in hello terminal.</span></span>

### <a name="commit-your-changes"></a><span data-ttu-id="17ba3-207">Genomför ändringarna</span><span class="sxs-lookup"><span data-stu-id="17ba3-207">Commit your changes</span></span>

<span data-ttu-id="17ba3-208">Kör hello följande Git-kommandon toocommit ändringarna:</span><span class="sxs-lookup"><span data-stu-id="17ba3-208">Run hello following Git commands toocommit your changes:</span></span>

```bash
git add .
git commit -m "database.php updates"
```

<span data-ttu-id="17ba3-209">Appen är redo toobe distribueras.</span><span class="sxs-lookup"><span data-stu-id="17ba3-209">Your app is ready toobe deployed.</span></span>

## <a name="deploy-tooazure"></a><span data-ttu-id="17ba3-210">Distribuera tooAzure</span><span class="sxs-lookup"><span data-stu-id="17ba3-210">Deploy tooAzure</span></span>

<span data-ttu-id="17ba3-211">I det här steget kan distribuera du hello MySQL-anslutna PHP programmet tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="17ba3-211">In this step, you deploy hello MySQL-connected PHP application tooAzure App Service.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="17ba3-212">Skapa en App Service-plan</span><span class="sxs-lookup"><span data-stu-id="17ba3-212">Create an App Service plan</span></span>

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a><span data-ttu-id="17ba3-213">Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="17ba3-213">Create a web app</span></span>

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)]

### <a name="set-hello-php-version"></a><span data-ttu-id="17ba3-214">Ange hello PHP-version</span><span class="sxs-lookup"><span data-stu-id="17ba3-214">Set hello PHP version</span></span>

<span data-ttu-id="17ba3-215">Ange hello PHP-version som hello program kräver via hello [az webapp konfigurationsuppsättning](/cli/azure/webapp/config#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="17ba3-215">Set hello PHP version that hello application requires by using hello [az webapp config set](/cli/azure/webapp/config#set) command.</span></span>

<span data-ttu-id="17ba3-216">hello anger följande kommando hello PHP version too_7.0_.</span><span class="sxs-lookup"><span data-stu-id="17ba3-216">hello following command sets hello PHP version too_7.0_.</span></span>

```azurecli-interactive
az webapp config set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a><span data-ttu-id="17ba3-217">Konfigurera databasinställningar för</span><span class="sxs-lookup"><span data-stu-id="17ba3-217">Configure database settings</span></span>

<span data-ttu-id="17ba3-218">Du kan ansluta tooyour Azure MySQL-databas med miljövariabler i App Service som pekas tidigare.</span><span class="sxs-lookup"><span data-stu-id="17ba3-218">As pointed out previously, you can connect tooyour Azure MySQL database using environment variables in App Service.</span></span>

<span data-ttu-id="17ba3-219">I App Service som du anger miljövariabler som _appinställningar_ med hjälp av hello [az webapp appsettings konfigurationsuppsättning](/cli/azure/webapp/config/appsettings#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="17ba3-219">In App Service, you set environment variables as _app settings_ by using hello [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span>

<span data-ttu-id="17ba3-220">hello följande kommando konfigurerar hello appinställningar `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, och `DB_PASSWORD`.</span><span class="sxs-lookup"><span data-stu-id="17ba3-220">hello following command configures hello app settings `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, and `DB_PASSWORD`.</span></span> <span data-ttu-id="17ba3-221">Ersätt platshållarna hello  _&lt;appname >_ och  _&lt;mysql_server_name >_.</span><span class="sxs-lookup"><span data-stu-id="17ba3-221">Replace hello placeholders _&lt;appname>_ and _&lt;mysql_server_name>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

<span data-ttu-id="17ba3-222">Du kan använda hello PHP [getenv](http://www.php.net/manual/function.getenv.php) metoden tooaccess hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="17ba3-222">You can use hello PHP [getenv](http://www.php.net/manual/function.getenv.php) method tooaccess hello settings.</span></span> <span data-ttu-id="17ba3-223">Hej Laravel koden använder en [env](https://laravel.com/docs/5.4/helpers#method-env) wrapper över hello PHP `getenv`.</span><span class="sxs-lookup"><span data-stu-id="17ba3-223">hello Laravel code uses an [env](https://laravel.com/docs/5.4/helpers#method-env) wrapper over hello PHP `getenv`.</span></span> <span data-ttu-id="17ba3-224">Till exempel hello MySQL konfigurationen i _config/database.php_ ser ut som följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="17ba3-224">For example, hello MySQL configuration in _config/database.php_ looks like hello following code:</span></span>

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

### <a name="configure-laravel-environment-variables"></a><span data-ttu-id="17ba3-225">Konfigurera Laravel miljövariabler</span><span class="sxs-lookup"><span data-stu-id="17ba3-225">Configure Laravel environment variables</span></span>

<span data-ttu-id="17ba3-226">Laravel måste en tangent i App Service.</span><span class="sxs-lookup"><span data-stu-id="17ba3-226">Laravel needs an application key in App Service.</span></span> <span data-ttu-id="17ba3-227">Du kan konfigurera den med app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="17ba3-227">You can configure it with app settings.</span></span>

<span data-ttu-id="17ba3-228">Använd `php artisan` toogenerate en ny nyckel för program utan att spara den too_.env_.</span><span class="sxs-lookup"><span data-stu-id="17ba3-228">Use `php artisan` toogenerate a new application key without saving it too_.env_.</span></span>

```bash
php artisan key:generate --show
```

<span data-ttu-id="17ba3-229">Ange hello tangent i hello Apptjänst webbprogram med hjälp av hello [az webapp konfigurationsuppsättning appsettings](/cli/azure/webapp/config/appsettings#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="17ba3-229">Set hello application key in hello App Service web app by using hello [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#set) command.</span></span> <span data-ttu-id="17ba3-230">Ersätt platshållarna hello  _&lt;appname >_ och  _&lt;outputofphpartisankey: Generera >_.</span><span class="sxs-lookup"><span data-stu-id="17ba3-230">Replace hello placeholders _&lt;appname>_ and _&lt;outputofphpartisankey:generate>_.</span></span>

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

<span data-ttu-id="17ba3-231">`APP_DEBUG="true"`Anger Laravel tooreturn felsökningsinformation när hello distribuerade webbapp påträffar ett fel.</span><span class="sxs-lookup"><span data-stu-id="17ba3-231">`APP_DEBUG="true"` tells Laravel tooreturn debugging information when hello deployed web app encounters errors.</span></span> <span data-ttu-id="17ba3-232">När du kör ett produktionsprogram ställa in den också`false`, vilket är säkrare.</span><span class="sxs-lookup"><span data-stu-id="17ba3-232">When running a production application, set it too`false`, which is more secure.</span></span>

### <a name="set-hello-virtual-application-path"></a><span data-ttu-id="17ba3-233">Ange sökväg för hello virtuella program</span><span class="sxs-lookup"><span data-stu-id="17ba3-233">Set hello virtual application path</span></span>

<span data-ttu-id="17ba3-234">Ange sökväg till hello virtuella program för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="17ba3-234">Set hello virtual application path for hello web app.</span></span> <span data-ttu-id="17ba3-235">Det här steget är nödvändigt eftersom hello [Laravel programmet livscykel](https://laravel.com/docs/5.4/lifecycle) börjar i hello _offentliga_ katalog i stället för hello tillämpningsprogrammets rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="17ba3-235">This step is required because hello [Laravel application lifecycle](https://laravel.com/docs/5.4/lifecycle) begins in hello _public_ directory instead of hello application's root directory.</span></span> <span data-ttu-id="17ba3-236">Andra PHP-ramverk vars livscykel startar i rotkatalogen för hello kan arbeta utan manuell konfiguration av hello virtuell sökväg.</span><span class="sxs-lookup"><span data-stu-id="17ba3-236">Other PHP frameworks whose lifecycle start in hello root directory can work without manual configuration of hello virtual application path.</span></span>

<span data-ttu-id="17ba3-237">Ange hello virtuella programmet sökväg med hjälp av hello [az resurs uppdaterades](/cli/azure/resource#update) kommando.</span><span class="sxs-lookup"><span data-stu-id="17ba3-237">Set hello virtual application path by using hello [az resource update](/cli/azure/resource#update) command.</span></span> <span data-ttu-id="17ba3-238">Ersätt hello  _&lt;appname >_ platshållare.</span><span class="sxs-lookup"><span data-stu-id="17ba3-238">Replace hello _&lt;appname>_ placeholder.</span></span>

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

<span data-ttu-id="17ba3-239">Som standard pekar hello rotsökvägen för virtuella program i Azure App Service (_/_) toohello rotkatalog hello distribueras programfiler (_sites\wwwroot_).</span><span class="sxs-lookup"><span data-stu-id="17ba3-239">By default, Azure App Service points hello root virtual application path (_/_) toohello root directory of hello deployed application files (_sites\wwwroot_).</span></span>

### <a name="configure-a-deployment-user"></a><span data-ttu-id="17ba3-240">Konfigurera en distributionsanvändare</span><span class="sxs-lookup"><span data-stu-id="17ba3-240">Configure a deployment user</span></span>

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="configure-local-git-deployment"></a><span data-ttu-id="17ba3-241">Konfigurera den lokala Git-distributionen</span><span class="sxs-lookup"><span data-stu-id="17ba3-241">Configure local Git deployment</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git-no-h.md)]

### <a name="push-tooazure-from-git"></a><span data-ttu-id="17ba3-242">Push-tooAzure från Git</span><span class="sxs-lookup"><span data-stu-id="17ba3-242">Push tooAzure from Git</span></span>

<span data-ttu-id="17ba3-243">Lägg till en Azure remote tooyour lokal Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="17ba3-243">Add an Azure remote tooyour local Git repository.</span></span>

```bash
git remote add azure <paste_copied_url_here>
```

<span data-ttu-id="17ba3-244">Skicka toohello Azure remote toodeploy hello PHP-program.</span><span class="sxs-lookup"><span data-stu-id="17ba3-244">Push toohello Azure remote toodeploy hello PHP application.</span></span> <span data-ttu-id="17ba3-245">Du uppmanas hello lösenordet du angav tidigare som en del av hello skapandet av hello distribution av användaren.</span><span class="sxs-lookup"><span data-stu-id="17ba3-245">You are prompted for hello password you supplied earlier as part of hello creation of hello deployment user.</span></span>

```bash
git push azure master
```

<span data-ttu-id="17ba3-246">Under distributionen kommunicerar förloppet med Git i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="17ba3-246">During deployment, Azure App Service communicates its progress with Git.</span></span>

```bash
Counting objects: 3, done.
Delta compression using up too8 threads.
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
> <span data-ttu-id="17ba3-247">Det kan hända att hello distributionsprocessen installerar [Composer](https://getcomposer.org/) paket hello slutet.</span><span class="sxs-lookup"><span data-stu-id="17ba3-247">You may notice that hello deployment process installs [Composer](https://getcomposer.org/) packages at hello end.</span></span> <span data-ttu-id="17ba3-248">App Service kör inte dessa automatiseringar under distributionen av standard, så att det här exemplet databasen har tre ytterligare filer i dess rot directory tooenable som:</span><span class="sxs-lookup"><span data-stu-id="17ba3-248">App Service does not run these automations during default deployment, so this sample repository has three additional files in its root directory tooenable it:</span></span>
>
> - <span data-ttu-id="17ba3-249">`.deployment`-Den här filen talar om Apptjänst toorun `bash deploy.sh` som hello distribution av anpassade skript.</span><span class="sxs-lookup"><span data-stu-id="17ba3-249">`.deployment` - This file tells App Service toorun `bash deploy.sh` as hello custom deployment script.</span></span>
> - <span data-ttu-id="17ba3-250">`deploy.sh`-hello anpassat distributionsskriptet.</span><span class="sxs-lookup"><span data-stu-id="17ba3-250">`deploy.sh` - hello custom deployment script.</span></span> <span data-ttu-id="17ba3-251">Om du har läst hello-fil, ser du att den körs `php composer.phar install` när `npm install`.</span><span class="sxs-lookup"><span data-stu-id="17ba3-251">If you review hello file, you will see that it runs `php composer.phar install` after `npm install`.</span></span>
> - <span data-ttu-id="17ba3-252">`composer.phar`-hello Composer Pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="17ba3-252">`composer.phar` - hello Composer package manager.</span></span>
>
> <span data-ttu-id="17ba3-253">Du kan använda den här metoden tooadd alla steg tooyour Git-baserad distribution tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="17ba3-253">You can use this approach tooadd any step tooyour Git-based deployment tooApp Service.</span></span> <span data-ttu-id="17ba3-254">Mer information finns i [anpassat distributionsskriptet](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span><span class="sxs-lookup"><span data-stu-id="17ba3-254">For more information, see [Custom Deployment Script](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).</span></span>
>

### <a name="browse-toohello-azure-web-app"></a><span data-ttu-id="17ba3-255">Bläddra toohello Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="17ba3-255">Browse toohello Azure web app</span></span>

<span data-ttu-id="17ba3-256">Bläddra för`http://<app_name>.azurewebsites.net` och lägga till några uppgifter toohello lista.</span><span class="sxs-lookup"><span data-stu-id="17ba3-256">Browse too`http://<app_name>.azurewebsites.net` and add a few tasks toohello list.</span></span>

![PHP-app som körs i Azure App Service](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

<span data-ttu-id="17ba3-258">Grattis, du kör en datadrivna PHP-app i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="17ba3-258">Congratulations, you're running a data-driven PHP app in Azure App Service.</span></span>

## <a name="update-model-locally-and-redeploy"></a><span data-ttu-id="17ba3-259">Uppdatera modellen lokalt och distribuera</span><span class="sxs-lookup"><span data-stu-id="17ba3-259">Update model locally and redeploy</span></span>

<span data-ttu-id="17ba3-260">I det här steget kan du göra en enkel förändring toohello `task` data modellen hello webapp och sedan publicera hello uppdatering tooAzure.</span><span class="sxs-lookup"><span data-stu-id="17ba3-260">In this step, you make a simple change toohello `task` data model and hello webapp, and then publish hello update tooAzure.</span></span>

<span data-ttu-id="17ba3-261">Hello uppgifter scenariot ändra hello program så att du kan markera en aktivitet som slutförd.</span><span class="sxs-lookup"><span data-stu-id="17ba3-261">For hello tasks scenario, you modify hello application so that you can mark a task as complete.</span></span>

### <a name="add-a-column"></a><span data-ttu-id="17ba3-262">Lägg till en kolumn</span><span class="sxs-lookup"><span data-stu-id="17ba3-262">Add a column</span></span>

<span data-ttu-id="17ba3-263">Navigera i hello terminal, toohello rot hello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="17ba3-263">In hello terminal, navigate toohello root of hello Git repository.</span></span>

<span data-ttu-id="17ba3-264">Generera en ny Databasmigrering för hello `tasks` tabell:</span><span class="sxs-lookup"><span data-stu-id="17ba3-264">Generate a new database migration for hello `tasks` table:</span></span>

```bash
php artisan make:migration add_complete_column --table=tasks
```

<span data-ttu-id="17ba3-265">Detta kommando visar hello av namnet på hello migreringsfilen som genereras.</span><span class="sxs-lookup"><span data-stu-id="17ba3-265">This command shows you hello name of hello migration file that's generated.</span></span> <span data-ttu-id="17ba3-266">Den här filen i _databasen/migreringar_ och öppna den.</span><span class="sxs-lookup"><span data-stu-id="17ba3-266">Find this file in _database/migrations_ and open it.</span></span>

<span data-ttu-id="17ba3-267">Ersätt hello `up` metod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="17ba3-267">Replace hello `up` method with hello following code:</span></span>

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

<span data-ttu-id="17ba3-268">hello föregående kod lägger till en boolesk kolumn i hello `tasks` tabell som kallas `complete`.</span><span class="sxs-lookup"><span data-stu-id="17ba3-268">hello preceding code adds a boolean column in hello `tasks` table called `complete`.</span></span>

<span data-ttu-id="17ba3-269">Ersätt hello `down` metod med följande kod för hello Återföringsåtgärd hello:</span><span class="sxs-lookup"><span data-stu-id="17ba3-269">Replace hello `down` method with hello following code for hello rollback action:</span></span>

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

<span data-ttu-id="17ba3-270">Kör Laravel migreringar toomake hello databasändring i hello terminal, i hello lokal databas.</span><span class="sxs-lookup"><span data-stu-id="17ba3-270">In hello terminal, run Laravel database migrations toomake hello change in hello local database.</span></span>

```bash
php artisan migrate
```

<span data-ttu-id="17ba3-271">Baserat på hello [Laravel namngivningskonvention](https://laravel.com/docs/5.4/eloquent#defining-models), hello modellen `Task` (se _app/Task.php_) mappar toohello `tasks` tabellen som standard.</span><span class="sxs-lookup"><span data-stu-id="17ba3-271">Based on hello [Laravel naming convention](https://laravel.com/docs/5.4/eloquent#defining-models), hello model `Task` (see _app/Task.php_) maps toohello `tasks` table by default.</span></span>

### <a name="update-application-logic"></a><span data-ttu-id="17ba3-272">Uppdatera programlogik</span><span class="sxs-lookup"><span data-stu-id="17ba3-272">Update application logic</span></span>

<span data-ttu-id="17ba3-273">Öppna hello *routes/web.php* fil.</span><span class="sxs-lookup"><span data-stu-id="17ba3-273">Open hello *routes/web.php* file.</span></span> <span data-ttu-id="17ba3-274">hello programmet definierar dess vägar och affärslogik här.</span><span class="sxs-lookup"><span data-stu-id="17ba3-274">hello application defines its routes and business logic here.</span></span>

<span data-ttu-id="17ba3-275">Hello slutet av hello-fil, lägger du till en väg med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="17ba3-275">At hello end of hello file, add a route with hello following code:</span></span>

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

<span data-ttu-id="17ba3-276">hello föregående kod gör en enkel update toohello datamodell genom att klicka hello värdet för `complete`.</span><span class="sxs-lookup"><span data-stu-id="17ba3-276">hello preceding code makes a simple update toohello data model by toggling hello value of `complete`.</span></span>

### <a name="update-hello-view"></a><span data-ttu-id="17ba3-277">Uppdatera hello vy</span><span class="sxs-lookup"><span data-stu-id="17ba3-277">Update hello view</span></span>

<span data-ttu-id="17ba3-278">Öppna hello *resources/views/tasks.blade.php* fil.</span><span class="sxs-lookup"><span data-stu-id="17ba3-278">Open hello *resources/views/tasks.blade.php* file.</span></span> <span data-ttu-id="17ba3-279">Hitta hello `<tr>` startkoden och Ersätt den med:</span><span class="sxs-lookup"><span data-stu-id="17ba3-279">Find hello `<tr>` opening tag and replace it with:</span></span>

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

<span data-ttu-id="17ba3-280">hello föregående kod ändrar hello raden färg beroende på om hello åtgärden är klar.</span><span class="sxs-lookup"><span data-stu-id="17ba3-280">hello preceding code changes hello row color depending on whether hello task is complete.</span></span>

<span data-ttu-id="17ba3-281">Hello nästa rad har du hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="17ba3-281">In hello next line, you have hello following code:</span></span>

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

<span data-ttu-id="17ba3-282">Ersätt hello hela raden med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="17ba3-282">Replace hello entire line with hello following code:</span></span>

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

<span data-ttu-id="17ba3-283">hello föregående kod lägger till hello Skicka-knapp som refererar till hello flödet som du angav tidigare.</span><span class="sxs-lookup"><span data-stu-id="17ba3-283">hello preceding code adds hello submit button that references hello route that you defined earlier.</span></span>

### <a name="test-hello-changes-locally"></a><span data-ttu-id="17ba3-284">Testa hello ändringar lokalt</span><span class="sxs-lookup"><span data-stu-id="17ba3-284">Test hello changes locally</span></span>

<span data-ttu-id="17ba3-285">Kör hello utvecklingsserver från hello rotkatalogen för hello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="17ba3-285">From hello root directory of hello Git repository, run hello development server.</span></span>

```bash
php artisan serve
```

<span data-ttu-id="17ba3-286">toosee hello uppgift status ändras, navigera för`http://localhost:8000` och välj hello kryssruta.</span><span class="sxs-lookup"><span data-stu-id="17ba3-286">toosee hello task status change, navigate too`http://localhost:8000` and select hello checkbox.</span></span>

![Tillagda kryssrutan tootask](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

<span data-ttu-id="17ba3-288">Skriv toostop PHP `Ctrl + C` i hello terminal.</span><span class="sxs-lookup"><span data-stu-id="17ba3-288">toostop PHP, type `Ctrl + C` in hello terminal.</span></span>

### <a name="publish-changes-tooazure"></a><span data-ttu-id="17ba3-289">Publicera ändringar tooAzure</span><span class="sxs-lookup"><span data-stu-id="17ba3-289">Publish changes tooAzure</span></span>

<span data-ttu-id="17ba3-290">I hello terminal, kör du Laravel databasen migreringar med hello produktion anslutning sträng toomake hello ändring i hello Azure-databas.</span><span class="sxs-lookup"><span data-stu-id="17ba3-290">In hello terminal, run Laravel database migrations with hello production connection string toomake hello change in hello Azure database.</span></span>

```bash
php artisan migrate --env=production --force
```

<span data-ttu-id="17ba3-291">Genomför alla hello ändringar i Git och skicka sedan hello kod ändringar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="17ba3-291">Commit all hello changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

<span data-ttu-id="17ba3-292">En gång hello `git push` är slutförd, navigera toohello Azure web app och testa hello nya funktioner.</span><span class="sxs-lookup"><span data-stu-id="17ba3-292">Once hello `git push` is complete, navigate toohello Azure web app and test hello new functionality.</span></span>

![Ändringar i modellen och databasen publicerade tooAzure](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

<span data-ttu-id="17ba3-294">Om du har lagt till alla uppgifter som finns kvar i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="17ba3-294">If you added any tasks, they are retained in hello database.</span></span> <span data-ttu-id="17ba3-295">Uppdateringar toohello dataschemat lämna befintliga data intakta.</span><span class="sxs-lookup"><span data-stu-id="17ba3-295">Updates toohello data schema leave existing data intact.</span></span>

## <a name="stream-diagnostic-logs"></a><span data-ttu-id="17ba3-296">Dataströmmen diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="17ba3-296">Stream diagnostic logs</span></span>

<span data-ttu-id="17ba3-297">När hello PHP-program körs i Azure App Service kan få du hello konsolen loggar via rörledningar tooyour terminal.</span><span class="sxs-lookup"><span data-stu-id="17ba3-297">While hello PHP application runs in Azure App Service, you can get hello console logs piped tooyour terminal.</span></span> <span data-ttu-id="17ba3-298">På så sätt kan du hello diagnostiska meddelanden för samma toohelp du felsöka programfel.</span><span class="sxs-lookup"><span data-stu-id="17ba3-298">That way, you can get hello same diagnostic messages toohelp you debug application errors.</span></span>

<span data-ttu-id="17ba3-299">toostart loggen direktuppspelning, Använd hello [az webapp loggen pilslut](/cli/azure/webapp/log#tail) kommando.</span><span class="sxs-lookup"><span data-stu-id="17ba3-299">toostart log streaming, use hello [az webapp log tail](/cli/azure/webapp/log#tail) command.</span></span>

```azurecli-interactive
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup
```

<span data-ttu-id="17ba3-300">Uppdatera hello Azure-webbapp i hello webbläsare tooget vissa webbtrafik när loggen streaming har startats.</span><span class="sxs-lookup"><span data-stu-id="17ba3-300">Once log streaming has started, refresh hello Azure web app in hello browser tooget some web traffic.</span></span> <span data-ttu-id="17ba3-301">Du kan nu se konsolen loggar via rörledningar toohello terminal.</span><span class="sxs-lookup"><span data-stu-id="17ba3-301">You can now see console logs piped toohello terminal.</span></span> <span data-ttu-id="17ba3-302">Om du inte ser loggarna för konsolen omedelbart, Kontrollera igen i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="17ba3-302">If you don't see console logs immediately, check again in 30 seconds.</span></span>

<span data-ttu-id="17ba3-303">toostop loggen strömning när som helst, typen `Ctrl` + `C`.</span><span class="sxs-lookup"><span data-stu-id="17ba3-303">toostop log streaming at anytime, type `Ctrl`+`C`.</span></span>

> [!TIP]
> <span data-ttu-id="17ba3-304">Ett PHP-program kan använda hello standard [error_log()](http://php.net/manual/function.error-log.php) toooutput toohello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="17ba3-304">A PHP application can use hello standard [error_log()](http://php.net/manual/function.error-log.php) toooutput toohello console.</span></span> <span data-ttu-id="17ba3-305">hello exempelprogrammet använder den här metoden i _app/Http/routes.php_.</span><span class="sxs-lookup"><span data-stu-id="17ba3-305">hello sample application uses this approach in _app/Http/routes.php_.</span></span>
>
> <span data-ttu-id="17ba3-306">Som ett webbramverk [Laravel använder Monolog](https://laravel.com/docs/5.4/errors) som hello loggning provider.</span><span class="sxs-lookup"><span data-stu-id="17ba3-306">As a web framework, [Laravel uses Monolog](https://laravel.com/docs/5.4/errors) as hello logging provider.</span></span> <span data-ttu-id="17ba3-307">toosee tooget Monolog toooutput felmeddelanden toohello-konsolen finns [PHP: hur toouse monolog toolog tooconsole (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span><span class="sxs-lookup"><span data-stu-id="17ba3-307">toosee how tooget Monolog toooutput messages toohello console, see [PHP: How toouse monolog toolog tooconsole (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).</span></span>
>
>

## <a name="manage-hello-azure-web-app"></a><span data-ttu-id="17ba3-308">Hantera hello Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="17ba3-308">Manage hello Azure web app</span></span>

<span data-ttu-id="17ba3-309">Gå toohello [Azure-portalen](https://portal.azure.com) toomanage hello webbprogram som du skapade.</span><span class="sxs-lookup"><span data-stu-id="17ba3-309">Go toohello [Azure portal](https://portal.azure.com) toomanage hello web app you created.</span></span>

<span data-ttu-id="17ba3-310">Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="17ba3-310">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-php-mysql/access-portal.png)

<span data-ttu-id="17ba3-312">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="17ba3-312">You see your web app's Overview page.</span></span> <span data-ttu-id="17ba3-313">Här kan du utföra grundläggande hanteringsuppgifter som att stoppa, starta, omstart, bläddra och ta bort.</span><span class="sxs-lookup"><span data-stu-id="17ba3-313">Here, you can perform basic management tasks like  stop, start, restart, browse, and delete.</span></span>

<span data-ttu-id="17ba3-314">hello vänstra menyn innehåller sidor för att konfigurera din app.</span><span class="sxs-lookup"><span data-stu-id="17ba3-314">hello left menu provides pages for configuring your app.</span></span>

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="17ba3-316">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17ba3-316">Next steps</span></span>

<span data-ttu-id="17ba3-317">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="17ba3-317">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="17ba3-318">Skapa en MySQL-databas i Azure</span><span class="sxs-lookup"><span data-stu-id="17ba3-318">Create a MySQL database in Azure</span></span>
> * <span data-ttu-id="17ba3-319">Ansluta en app tooMySQL för PHP</span><span class="sxs-lookup"><span data-stu-id="17ba3-319">Connect a PHP app tooMySQL</span></span>
> * <span data-ttu-id="17ba3-320">Distribuera hello app tooAzure</span><span class="sxs-lookup"><span data-stu-id="17ba3-320">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="17ba3-321">Uppdatera hello-datamodell och omdistribuera hello app</span><span class="sxs-lookup"><span data-stu-id="17ba3-321">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="17ba3-322">Dataströmmen diagnostiska loggar från Azure</span><span class="sxs-lookup"><span data-stu-id="17ba3-322">Stream diagnostic logs from Azure</span></span>
> * <span data-ttu-id="17ba3-323">Hantera hello appen i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="17ba3-323">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="17ba3-324">I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn tooa webbprogram.</span><span class="sxs-lookup"><span data-stu-id="17ba3-324">Advance toohello next tutorial toolearn how toomap a custom DNS name tooa web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="17ba3-325">Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps</span><span class="sxs-lookup"><span data-stu-id="17ba3-325">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
