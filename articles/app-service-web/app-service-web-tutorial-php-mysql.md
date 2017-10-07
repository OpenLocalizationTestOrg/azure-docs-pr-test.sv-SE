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
# <a name="build-a-php-and-mysql-web-app-in-azure"></a>Skapa en PHP- och MySQL-webbapp i Azure

Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst. Den här kursen visar hur toocreate en PHP webbapp i Azure och koppla den tooa MySQL-databas. När du är klar har du en [Laravel](https://laravel.com/) app som körs på Azure App Service Web Apps.

![PHP-app som körs i Azure App Service](./media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Skapa en MySQL-databas i Azure
> * Ansluta en app tooMySQL för PHP
> * Distribuera hello app tooAzure
> * Uppdatera hello-datamodell och omdistribuera hello app
> * Dataströmmen diagnostiska loggar från Azure
> * Hantera hello appen i hello Azure-portalen

## <a name="prerequisites"></a>Krav

toocomplete den här kursen:

* [Installera Git](https://git-scm.com/)
* [Installera PHP 5.6.4 eller senare](http://php.net/downloads.php)
* [Installera Composer](https://getcomposer.org/doc/00-intro.md)
* Aktivera följande PHP-tillägg Laravel behov hello: OpenSSL, PDO MySQL, Mbstring, Tokenizer, XML
* [Installera och starta MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Förbereda lokala MySQL

I det här steget kan skapa du en databas i din lokala MySQL-server för användning i den här kursen.

### <a name="connect-toolocal-mysql-server"></a>Anslut toolocal MySQL-server

Anslut tooyour lokala MySQL-servern i ett terminalfönster. Du kan använda den här terminalfönster toorun alla hello-kommandon i den här självstudiekursen.

```bash
mysql -u root -p
```

Om du uppmanas att ange ett lösenord anger hello lösenordet för hello `root` konto. Om du inte kommer ihåg rotlösenordet, se [MySQL: hur tooReset hello Rotlösenordet](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Om kommandot körs utan problem, är MySQL-servern igång. Om inte, kontrollera att den lokala MySQL-servern har startats med följande hello [MySQL efter installationssteg](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database-locally"></a>Skapa en databas lokalt

Vid hello `mysql` uppmanar, skapa en databas.

```sql 
CREATE DATABASE sampledb;
```

Avsluta anslutningen till servern genom att skriva `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a>Skapa en PHP-app lokalt
I det här steget hämta ett Laravel exempelprogram, konfigurera dess databasanslutningen och köra det lokalt. 

### <a name="clone-hello-sample"></a>Klona hello-exempel

I hello terminalfönster, `cd` tooa arbetskatalogen.

Hello kör följande kommando tooclone hello exempel lagringsplatsen.

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

`cd`tooyour klonade katalog.
Installera hello krävs paket.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>Konfigurera MySQL-anslutning

Skapa en fil med namnet i hello Lagringsplatsens rot *.env*. Kopiera hello följande variabler i hello *.env* fil. Ersätt hello  _&lt;root_password >_ med hello MySQL rotanvändarens lösenord.

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

Mer information om hur Laravel använder hello _.env_ fil, se [Laravel miljö Configuration](https://laravel.com/docs/5.4/configuration#environment-configuration).

### <a name="run-hello-sample-locally"></a>Kör hello exempel lokalt

Kör [Laravel databasen migreringar](https://laravel.com/docs/5.4/migrations) toocreate hello tabeller hello programbehov. vilka tabeller skapas hello migreringar titta i hello toosee _databasen/migreringar_ katalogen i hello Git-lagringsplats.

```bash
php artisan migrate
```

Skapa en ny Laravel programmet nyckel.

```bash
php artisan key:generate
```

Kör hello program.

```bash
php artisan serve
```

Navigera för`http://localhost:8000` i en webbläsare. Lägga till några åtgärder på hello-sidan.

![PHP ansluter har tooMySQL](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Skriv toostop PHP `Ctrl + C` i hello terminal.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Skapa MySQL i Azure

I det här steget skapar du en MySQL-databas i [Azure-databas för MySQL (förhandsgranskning)](/azure/mysql). Senare kan konfigurera du hello PHP tooconnect toothis databas.

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Skapa en MySQL-server

Skapa en server i Azure-databas för MySQL (förhandsversion) med hello [az mysql-servern skapa](/cli/azure/mysql/server#create) kommando.

Hello följande kommando, Ersätt namnet på MySQL-servern där du ser hello  _&lt;mysql_server_name >_ platshållare (giltiga tecken är `a-z`, `0-9`, och `-`). Det här namnet är en del av hello MySQL serverns värdnamn (`<mysql_server_name>.database.windows.net`), den måste toobe globalt unika.

```azurecli-interactive
az mysql server create \
    --name <mysql_server_name> \
    --resource-group myResourceGroup \
    --location "North Europe" \
    --admin-user adminuser \
    --admin-password MySQLAzure2017
```

När hello MySQL server skapas visar hello Azure CLI information liknande toohello följande exempel:

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

### <a name="configure-server-firewall"></a>Konfigurera server-brandväggen

Skapa en brandväggsregel för MySQL server tooallow klienten anslutningar med hello [az mysql-brandväggsregel skapa](/cli/azure/mysql/server/firewall-rule#create) kommando.

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name> \
    --resource-group myResourceGroup \
    --start-ip-address 0.0.0.0 \
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure-databas för MySQL (förhandsgranskning) begränsa inte för närvarande anslutningar endast tooAzure tjänster. IP-adresser i Azure tilldelas dynamiskt, är det bättre tooenable alla IP-adresser. hello-tjänsten är i förhandsgranskning. Bättre metoder för att skydda databasen planeras.
>
>

### <a name="connect-tooproduction-mysql-server-locally"></a>Ansluta tooproduction MySQL server lokalt

Hello terminalfönster, Anslut toohello MySQL-server i Azure. Använd hello-värde som du angav tidigare för  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

När du uppmanas att ange ett lösenord, Använd _tr0ngPa $$ w0rd!_, som du angav när du skapade hello-databasen.

### <a name="create-a-production-database"></a>Skapa en produktionsdatabas

Vid hello `mysql` uppmanar, skapa en databas.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Skapa en användare med behörighet

Skapa en databasanvändare som kallas _phpappuser_ och ge den alla behörigheter i hello `sampledb` databas.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* too'phpappuser';
```

Avsluta hello serveranslutning genom att skriva `quit`.

```sql
quit
```

## <a name="connect-app-tooazure-mysql"></a>Ansluta appen tooAzure MySQL

I det här steget kan ansluta du hello PHP programmet toohello MySQL-databas du skapade i Azure-databas för MySQL (förhandsversion).

<a name="devconfig"></a>

### <a name="configure-hello-database-connection"></a>Konfigurera hello databasanslutning

I Lagringsplatsens rot hello, skapar en _. env.production_ filen och kopiera hello följande variabler i den. Ersätt platshållaren hello  _&lt;mysql_server_name >_.

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

Spara hello ändringar.

> [!TIP]
> toosecure MySQL anslutningsinformationen, den här filen har redan exkluderats från hello Git-lagringsplats (se _.gitignore_ i hello Lagringsplatsens rot). Senare kan du lära dig hur tooconfigure miljövariabler i Apptjänst tooconnect tooyour databasen i Azure-databas för MySQL (förhandsversion). Med miljövariabler, behöver du inte hello *.env* filen i App Service.
>

### <a name="configure-ssl-certificate"></a>Konfigurera SSL-certifikat

Som standard tillämpar Azure-databas för MySQL SSL-anslutningar från klienter. tooconnect tooyour MySQL-databas i Azure, måste du använda en _.pem_ SSL-certifikat.

Öppna _config/database.php_ och Lägg till hello _sslmode_ och _alternativ_ parametrar för`connections.mysql`som visas i följande kod hello.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

toolearn hur toogenerate detta _certificate.pem_, se [Konfigurera SSL-anslutning i ditt program toosecurely ansluta tooAzure databas för MySQL](../mysql/howto-configure-ssl.md).

> [!TIP]
> hello sökvägen _/ssl/certificate.pem_ pekar tooan befintliga _certificate.pem_ filen i hello Git-lagringsplats. Den här filen finns i informationssyfte i den här självstudiekursen. För bästa praxis bör du inte utföra din _.pem_ certifikat till källkontroll. 
>

### <a name="test-hello-application-locally"></a>Testa hello programmet lokalt

Kör Laravel databasen migreringar med _. env.production_ som hello miljö filen toocreate hello tabeller i MySQL-databas i Azure-databas för MySQL (förhandsversion). Kom ihåg att _. env.production_ har hello anslutning information tooyour MySQL-databas i Azure.

```bash
php artisan migrate --env=production --force
```

_. env.production_ inte redan har en giltig App-nyckel. Generera en ny för det i hello terminal.

```bash
php artisan key:generate --env=production --force
```

Köra hello exempelprogrammet med _. env.production_ som hello miljö fil.

```bash
php artisan serve --env=production
```

Navigera för`http://localhost:8000`. Om hello sidan läses in utan fel ansluter hello PHP-program toohello MySQL-databas i Azure.

Lägga till några åtgärder på hello-sidan.

![PHP ansluter har tooAzure databas för MySQL (förhandsgranskning)](./media/app-service-web-tutorial-php-mysql/mysql-connect-success.png)

Skriv toostop PHP `Ctrl + C` i hello terminal.

### <a name="commit-your-changes"></a>Genomför ändringarna

Kör hello följande Git-kommandon toocommit ändringarna:

```bash
git add .
git commit -m "database.php updates"
```

Appen är redo toobe distribueras.

## <a name="deploy-tooazure"></a>Distribuera tooAzure

I det här steget kan distribuera du hello MySQL-anslutna PHP programmet tooAzure Apptjänst.

### <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

[!INCLUDE [Create app service plan no h](../../includes/app-service-web-create-app-service-plan-no-h.md)]

### <a name="create-a-web-app"></a>Skapa en webbapp

[!INCLUDE [Create web app no h](../../includes/app-service-web-create-web-app-no-h.md)]

### <a name="set-hello-php-version"></a>Ange hello PHP-version

Ange hello PHP-version som hello program kräver via hello [az webapp konfigurationsuppsättning](/cli/azure/webapp/config#set) kommando.

hello anger följande kommando hello PHP version too_7.0_.

```azurecli-interactive
az webapp config set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --php-version 7.0
```

### <a name="configure-database-settings"></a>Konfigurera databasinställningar för

Du kan ansluta tooyour Azure MySQL-databas med miljövariabler i App Service som pekas tidigare.

I App Service som du anger miljövariabler som _appinställningar_ med hjälp av hello [az webapp appsettings konfigurationsuppsättning](/cli/azure/webapp/config/appsettings#set) kommando.

hello följande kommando konfigurerar hello appinställningar `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, och `DB_PASSWORD`. Ersätt platshållarna hello  _&lt;appname >_ och  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017" MYSQL_SSL="true"
```

Du kan använda hello PHP [getenv](http://www.php.net/manual/function.getenv.php) metoden tooaccess hello inställningar. Hej Laravel koden använder en [env](https://laravel.com/docs/5.4/helpers#method-env) wrapper över hello PHP `getenv`. Till exempel hello MySQL konfigurationen i _config/database.php_ ser ut som följande kod hello:

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

### <a name="configure-laravel-environment-variables"></a>Konfigurera Laravel miljövariabler

Laravel måste en tangent i App Service. Du kan konfigurera den med app-inställningar.

Använd `php artisan` toogenerate en ny nyckel för program utan att spara den too_.env_.

```bash
php artisan key:generate --show
```

Ange hello tangent i hello Apptjänst webbprogram med hjälp av hello [az webapp konfigurationsuppsättning appsettings](/cli/azure/webapp/config/appsettings#set) kommando. Ersätt platshållarna hello  _&lt;appname >_ och  _&lt;outputofphpartisankey: Generera >_.

```azurecli-interactive
az webapp config appsettings set \
    --name <app_name> \
    --resource-group myResourceGroup \
    --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"`Anger Laravel tooreturn felsökningsinformation när hello distribuerade webbapp påträffar ett fel. När du kör ett produktionsprogram ställa in den också`false`, vilket är säkrare.

### <a name="set-hello-virtual-application-path"></a>Ange sökväg för hello virtuella program

Ange sökväg till hello virtuella program för hello webbprogrammet. Det här steget är nödvändigt eftersom hello [Laravel programmet livscykel](https://laravel.com/docs/5.4/lifecycle) börjar i hello _offentliga_ katalog i stället för hello tillämpningsprogrammets rotkatalog. Andra PHP-ramverk vars livscykel startar i rotkatalogen för hello kan arbeta utan manuell konfiguration av hello virtuell sökväg.

Ange hello virtuella programmet sökväg med hjälp av hello [az resurs uppdaterades](/cli/azure/resource#update) kommando. Ersätt hello  _&lt;appname >_ platshållare.

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

Som standard pekar hello rotsökvägen för virtuella program i Azure App Service (_/_) toohello rotkatalog hello distribueras programfiler (_sites\wwwroot_).

### <a name="configure-a-deployment-user"></a>Konfigurera en distributionsanvändare

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="configure-local-git-deployment"></a>Konfigurera den lokala Git-distributionen

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git-no-h.md)]

### <a name="push-tooazure-from-git"></a>Push-tooAzure från Git

Lägg till en Azure remote tooyour lokal Git-lagringsplats.

```bash
git remote add azure <paste_copied_url_here>
```

Skicka toohello Azure remote toodeploy hello PHP-program. Du uppmanas hello lösenordet du angav tidigare som en del av hello skapandet av hello distribution av användaren.

```bash
git push azure master
```

Under distributionen kommunicerar förloppet med Git i Azure App Service.

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
> Det kan hända att hello distributionsprocessen installerar [Composer](https://getcomposer.org/) paket hello slutet. App Service kör inte dessa automatiseringar under distributionen av standard, så att det här exemplet databasen har tre ytterligare filer i dess rot directory tooenable som:
>
> - `.deployment`-Den här filen talar om Apptjänst toorun `bash deploy.sh` som hello distribution av anpassade skript.
> - `deploy.sh`-hello anpassat distributionsskriptet. Om du har läst hello-fil, ser du att den körs `php composer.phar install` när `npm install`.
> - `composer.phar`-hello Composer Pakethanteraren.
>
> Du kan använda den här metoden tooadd alla steg tooyour Git-baserad distribution tooApp Service. Mer information finns i [anpassat distributionsskriptet](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
>

### <a name="browse-toohello-azure-web-app"></a>Bläddra toohello Azure-webbapp

Bläddra för`http://<app_name>.azurewebsites.net` och lägga till några uppgifter toohello lista.

![PHP-app som körs i Azure App Service](./media/app-service-web-tutorial-php-mysql/php-mysql-in-azure.png)

Grattis, du kör en datadrivna PHP-app i Azure App Service.

## <a name="update-model-locally-and-redeploy"></a>Uppdatera modellen lokalt och distribuera

I det här steget kan du göra en enkel förändring toohello `task` data modellen hello webapp och sedan publicera hello uppdatering tooAzure.

Hello uppgifter scenariot ändra hello program så att du kan markera en aktivitet som slutförd.

### <a name="add-a-column"></a>Lägg till en kolumn

Navigera i hello terminal, toohello rot hello Git-lagringsplats.

Generera en ny Databasmigrering för hello `tasks` tabell:

```bash
php artisan make:migration add_complete_column --table=tasks
```

Detta kommando visar hello av namnet på hello migreringsfilen som genereras. Den här filen i _databasen/migreringar_ och öppna den.

Ersätt hello `up` metod med hello följande kod:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

hello föregående kod lägger till en boolesk kolumn i hello `tasks` tabell som kallas `complete`.

Ersätt hello `down` metod med följande kod för hello Återföringsåtgärd hello:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

Kör Laravel migreringar toomake hello databasändring i hello terminal, i hello lokal databas.

```bash
php artisan migrate
```

Baserat på hello [Laravel namngivningskonvention](https://laravel.com/docs/5.4/eloquent#defining-models), hello modellen `Task` (se _app/Task.php_) mappar toohello `tasks` tabellen som standard.

### <a name="update-application-logic"></a>Uppdatera programlogik

Öppna hello *routes/web.php* fil. hello programmet definierar dess vägar och affärslogik här.

Hello slutet av hello-fil, lägger du till en väg med hello följande kod:

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

hello föregående kod gör en enkel update toohello datamodell genom att klicka hello värdet för `complete`.

### <a name="update-hello-view"></a>Uppdatera hello vy

Öppna hello *resources/views/tasks.blade.php* fil. Hitta hello `<tr>` startkoden och Ersätt den med:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

hello föregående kod ändrar hello raden färg beroende på om hello åtgärden är klar.

Hello nästa rad har du hello följande kod:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Ersätt hello hela raden med hello följande kod:

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

hello föregående kod lägger till hello Skicka-knapp som refererar till hello flödet som du angav tidigare.

### <a name="test-hello-changes-locally"></a>Testa hello ändringar lokalt

Kör hello utvecklingsserver från hello rotkatalogen för hello Git-lagringsplats.

```bash
php artisan serve
```

toosee hello uppgift status ändras, navigera för`http://localhost:8000` och välj hello kryssruta.

![Tillagda kryssrutan tootask](./media/app-service-web-tutorial-php-mysql/complete-checkbox.png)

Skriv toostop PHP `Ctrl + C` i hello terminal.

### <a name="publish-changes-tooazure"></a>Publicera ändringar tooAzure

I hello terminal, kör du Laravel databasen migreringar med hello produktion anslutning sträng toomake hello ändring i hello Azure-databas.

```bash
php artisan migrate --env=production --force
```

Genomför alla hello ändringar i Git och skicka sedan hello kod ändringar tooAzure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

En gång hello `git push` är slutförd, navigera toohello Azure web app och testa hello nya funktioner.

![Ändringar i modellen och databasen publicerade tooAzure](media/app-service-web-tutorial-php-mysql/complete-checkbox-published.png)

Om du har lagt till alla uppgifter som finns kvar i hello-databasen. Uppdateringar toohello dataschemat lämna befintliga data intakta.

## <a name="stream-diagnostic-logs"></a>Dataströmmen diagnostikloggar

När hello PHP-program körs i Azure App Service kan få du hello konsolen loggar via rörledningar tooyour terminal. På så sätt kan du hello diagnostiska meddelanden för samma toohelp du felsöka programfel.

toostart loggen direktuppspelning, Använd hello [az webapp loggen pilslut](/cli/azure/webapp/log#tail) kommando.

```azurecli-interactive
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup
```

Uppdatera hello Azure-webbapp i hello webbläsare tooget vissa webbtrafik när loggen streaming har startats. Du kan nu se konsolen loggar via rörledningar toohello terminal. Om du inte ser loggarna för konsolen omedelbart, Kontrollera igen i 30 sekunder.

toostop loggen strömning när som helst, typen `Ctrl` + `C`.

> [!TIP]
> Ett PHP-program kan använda hello standard [error_log()](http://php.net/manual/function.error-log.php) toooutput toohello-konsolen. hello exempelprogrammet använder den här metoden i _app/Http/routes.php_.
>
> Som ett webbramverk [Laravel använder Monolog](https://laravel.com/docs/5.4/errors) som hello loggning provider. toosee tooget Monolog toooutput felmeddelanden toohello-konsolen finns [PHP: hur toouse monolog toolog tooconsole (php://out)](http://stackoverflow.com/questions/25787258/php-how-to-use-monolog-to-log-to-console-php-out).
>
>

## <a name="manage-hello-azure-web-app"></a>Hantera hello Azure-webbapp

Gå toohello [Azure-portalen](https://portal.azure.com) toomanage hello webbprogram som du skapade.

Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-php-mysql/access-portal.png)

Nu visas sidan Översikt för din webbapp. Här kan du utföra grundläggande hanteringsuppgifter som att stoppa, starta, omstart, bläddra och ta bort.

hello vänstra menyn innehåller sidor för att konfigurera din app.

![App Service-sidan på Azure Portal](./media/app-service-web-tutorial-php-mysql/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa en MySQL-databas i Azure
> * Ansluta en app tooMySQL för PHP
> * Distribuera hello app tooAzure
> * Uppdatera hello-datamodell och omdistribuera hello app
> * Dataströmmen diagnostiska loggar från Azure
> * Hantera hello appen i hello Azure-portalen

I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn tooa webbprogram.

> [!div class="nextstepaction"]
> [Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](app-service-web-tutorial-custom-domain.md)
