---
title: "Använda DevOps miljöer effektivt för webbappen | Microsoft Docs"
description: "Lär dig hur du använder distributionsplatser för att konfigurera och hantera flera utvecklingsmiljöer för ditt program"
services: app-service\web
documentationcenter: 
author: sunbuild
manager: yochayk
editor: 
ms.assetid: 16a594dc-61f5-4984-b5ca-9d5abc39fb1e
ms.service: app-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 10/24/2016
ms.author: sumuth
ms.openlocfilehash: 25248411659f6c7b2e386e310428c365c44ea2e0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Använda DevOps miljöer effektivt för ditt webbprogram
Den här artikeln visar hur du ställer in och hantera web application-distributioner om flera versioner av programmet finns i olika miljöer, till exempel utveckling, mellanlagring, quality assurance (QA) och produktion. Varje version av programmet kan betraktas som en utvecklingsmiljö syfte av distributionsprocessen. Utvecklare kan använda QA-miljö för att testa kvaliteten på programmet innan de genomför ändringarna i produktion.
Flera utvecklingsmiljöer kan vara en utmaning eftersom du behöver spåra kod, hantera resurser (beräkning, webbprogram, databas, cache o.s.v.) och distribuera kod mellan miljöer.

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a>Konfigurera en icke-produktionsmiljö (steg, utveckling, QA)
När ett webbprogram för produktion är igång, är nästa steg att skapa en icke-produktionsmiljö. Kontrollera att du kör i läget Standard eller Premium Azure App Service-plan för att använda distributionsplatser. Distributionsplatser är live-webb-appar som har sina egna värdnamn. Web app innehåll och konfiguration element kan växlas mellan två distributionsplatser, inklusive produktionsplatsen. När du distribuerar ditt program till en distributionsplats kan få du följande fördelar:

- Du kan validera ändringar av en webbapp i en distribution mellanlagringsplatsen innan du byter appen med produktionsplatsen.
- När du först distribuera en webbapp till en plats och växla till produktion, är alla instanser av plats varmkörts innan som ska växlas över till produktion. Den här processen eliminerar avbrott när du distribuerar ditt webbprogram. Trafik för omdirigering är sömlös och inga begäranden tas bort på grund av byte åtgärder. Om du vill automatisera hela arbetsflödet, konfigurera [automatiskt växla](web-sites-staged-publishing.md#configure-auto-swap) när före växlingen verifiering inte behövs.
- Efter en växling har det fack som har tidigare mellanlagrade webbprogrammet nu tidigare webbprogrammet för produktion. Om ändringarna växlas över till produktionsplatsen är inte som du förväntade dig, kan du utföra samma växlingen direkt för att få din ”senast fungerande konfiguration” web app igen.

Om du vill konfigurera en distribution mellanlagringsplatsen finns [skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md). Alla miljöer bör innehålla en egen uppsättning resurser. Till exempel om ditt program använder en databas, ska sedan produktion och mellanlagring webbprogram använda olika databaser. Lägga till fristående development miljö resurser som databasen, lagring eller cacheminnet för att ange din fristående utvecklingsmiljö.

## <a name="examples-of-using-multiple-development-environments"></a>Exempel på användning av flera utvecklingsmiljöer
Alla projekt bör följa källa Kodhantering med minst två miljöer: utveckling och produktion. Om du använder innehållshanteringssystem (CMSs), ramverk för programmet, etc., kan programmet inte stöd för det här scenariot utan anpassning. Denna situation är true för några av populära ramverk som beskrivs i följande avsnitt. Många frågor kommer att tänka på när du arbetar med CMS/ramverk, som:

- Hur du bryter innehållet till olika miljöer?
- Vilka filer kan du ändra utan att påverka framework versionuppdateringar?
- Hur hanterar du konfigurationer per miljön?
- Hur hanterar du versionuppdateringar för moduler, plugin-program och centralt ramverk?

Det finns många sätt att konfigurera flera miljöer för projektet. I följande exempel visas en metod för varje respektive program.

### <a name="wordpress"></a>WordPress
I det här avsnittet får du lära dig hur du ställer in ett arbetsflöde för distribution med platser för WordPress. WordPress, stöder som de flesta CMS-lösningar, inte flera utvecklingsmiljöer utan anpassning. Funktionen Web Apps i Azure App Service har några funktioner som gör det enkelt att spara konfigurationsinställningarna utanför din kod.

1. Innan du skapar en mellanlagringsplatsen kan du ställa in din programkod för att stödja flera miljöer. För att stödja flera miljöer i WordPress, måste du redigera `wp-config.php` på din lokala development web app och Lägg till följande kod i början av filen. Den här processen kan ditt program att välja rätt konfiguration baserat på den valda miljön.

    ```
    // Support multiple environments
    // set the config file based on current environment
    if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
    // local development
     $config_file = 'config/wp-config.local.php';
    }
    elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
    //single file for all azure development environments
     $config_file = 'config/wp-config.azure.php';
    }
    $path = dirname(__FILE__). '/';
    if (file_exists($path. $config_file)) {
    // include the config file if it exists, otherwise WP is going to fail
    require_once $path. $config_file;
    ```

2. Skapa en mapp under web app rot kallas `config`, och Lägg till den `wp-config.azure.php` och `wp-config.local.php` filer som representerar dina Azure-miljön och den lokala miljön respektive.

3. Kopiera följande i `wp-config.local.php`:

    ```
    <?php
    // MySQL settings
    /** The name of the database for WordPress */

    define('DB_NAME', 'yourdatabasename');

    /** MySQL database username */
    define('DB_USER', 'yourdbuser');

    /** MySQL database password */
    define('DB_PASSWORD', 'yourpassword');

    /** MySQL hostname */
    define('DB_HOST', 'localhost');
    /**
     * For developers: WordPress debugging mode.
     * * Change this to true to enable the display of notices during development.
     * It is strongly recommended that plugin and theme developers use WP_DEBUG
     * in their development environments.
     */
    define('WP_DEBUG', true);

    //Security key settings
    define('AUTH_KEY', 'put your unique phrase here');
    define('SECURE_AUTH_KEY','put your unique phrase here');
    define('LOGGED_IN_KEY','put your unique phrase here');
    define('NONCE_KEY', 'put your unique phrase here');
    define('AUTH_SALT', 'put your unique phrase here');
    define('SECURE_AUTH_SALT', 'put your unique phrase here');
    define('LOGGED_IN_SALT', 'put your unique phrase here');
    define('NONCE_SALT', 'put your unique phrase here');

    /**
     * WordPress Database Table prefix.
     *
     * You can have multiple installations in one database if you give each a unique
     * prefix. Only numbers, letters, and underscores please!
     */
    $table_prefix = 'wp_';
    ```

    Inställningen säkerheten nycklar enligt beskrivningen i föregående kod kan hjälpa för att förhindra att ditt webbprogram som över, så Använd unika värden. Om du behöver skapa en sträng för säkerhetsnycklar anges i koden, kan du [gå till automatisk generator](https://api.wordpress.org/secret-key/1.1/salt) att skapa ny nyckel/värde-par.

4. Kopiera följande kod i `wp-config.azure.php`:

    ```    
    <?php
    // MySQL settings
    /** The name of the database for WordPress */

    define('DB_NAME', getenv('DB_NAME'));

    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));

    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));

    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));

    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */

    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);

    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));

    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
    ```

#### <a name="use-relative-paths"></a>Använda relativa sökvägar
En sista åtgärd för att konfigurera i WordPress-appen är relativa sökvägar. WordPress lagrar URL-information i databasen. Den här lagringsmedia kan flytta innehåll från en miljö till en annan svårare. Du måste uppdatera databasen för varje gång du flyttar från lokal till steget eller mellanlagra till produktionsmiljöer. Om du vill minska risken för problem som kan uppstå när du distribuerar en databas varje gång du distribuerar från en miljö till en annan, använder den [rot relativa länkar plugin-programmet](https://wordpress.org/plugins/root-relative-urls/), som du kan installera med hjälp av instrumentpanelen för WordPress-administratör.

Lägga till följande poster i din `wp-config.php` filen innan den `That's all, stop editing!` kommentar:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktivera plugin-programmet via den `Plugins` -menyn i instrumentpanelen i WordPress-administratören. Spara inställningarna webbadress för WordPress-appen.

#### <a name="the-final-wp-configphp-file"></a>Slutliga `wp-config.php` fil
Alla viktiga uppdateringar av WordPress påverkar inte din `wp-config.php`, `wp-config.azure.php`, och `wp-config.local.php` filer. Här är en slutlig version av den `wp-config.php` filen:

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Konfigurera en mellanlagringsmiljön
1. Om du redan har en WordPress-webbapp som körs på Azure-prenumerationen, logga in på den [Azure-portalen](http://portal.azure.com), och gå sedan till din WordPress-webbapp. Om du inte har en WordPress-webbappen kan skapa du en från Azure Marketplace. Läs mer i [skapa en WordPress-webbapp i Azure App Service](web-sites-php-web-site-gallery.md).
Klicka på **inställningar** > **distributionsfack** > **Lägg till** att skapa en distributionsplats med namnet *steget*. En distributionsplats är ett annat webbprogram som delar samma resurser som den primära webbapp som du skapade tidigare.

    ![Skapa steget distributionsplats](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. Lägg till en annan MySQL-databas, säg `wordpress-stage-db`, till en resursgrupp, `wordpressapp-group`.

    ![Lägga till MySQL-databas i resursgrupp](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. Uppdatera anslutningssträngarna för din steget distributionsplatsen så att den pekar till den nya databasen `wordpress-stage-db`. Webbappen produktion `wordpressprodapp`, och organisering webbapp `wordpressprodapp-stage`, måste peka på olika databaser.

#### <a name="configure-environment-specific-app-settings"></a>Konfigurera appinställningar för miljö-specifik
Utvecklare kan lagra nyckel/värde-par för sträng i Azure som en del av den konfigurationsinformation som kallas **Appinställningar**, som är kopplad till ett webbprogram. Vid körning, webbprogram automatiskt hämtar dessa värden och göra dem tillgängliga för kod som körs i ditt webbprogram. Från ett säkerhetsperspektiv som är en bra sida fördel eftersom känslig information, till exempel databasanslutningssträngar som innehåller lösenord, aldrig visas i klartext i en fil som `wp-config.php`.

Den här processen som beskrivs i följande stycken är användbar eftersom den innehåller både ändras och ändringar i databasen för WordPress-appen:

* WordPress versionsuppgradering
* Lägga till nya eller redigera eller uppgradera plugin-program
* Lägga till nya eller redigera eller uppgradera teman

Konfigurera appinställningar för:

* Information om databas
* Aktivera/inaktivera WordPress-loggning
* Säkerhetsinställningar för WordPress

![App-inställningar för Wordpress-webbapp](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Kontrollera att du lägger till följande appinställningar för din web app och fas produktionsplatsen. Observera att produktion webbapp och fristående webbprogrammet använda olika databaser.

1. Avmarkera den **fack inställningen** kryssrutan för alla inställningar parametrar utom WP_ENV. Den här processen kommer växla konfigurationen för din webbapp, innehåll och databasen. Om **fack inställningen** är markerat, måste webbappen app-inställningar och anslutningssträng konfiguration *inte* flytta mellan miljöer när du gör en **växla** igen. Alla ändringar i databasen som finns bryter inte ditt webbprogram för produktion.

2. Distribuera webbprogram för lokal utveckling miljö till steget webbapp och databasen med hjälp av WebMatrix eller -verktyg, till exempel FTP, Git eller PhpMyAdmin.

    ![Web Matrix publicera dialogrutan för WordPress-webbapp](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. Bläddra och testa din fristående webbapp. Med tanke på ett scenario där tema för webbappen är uppdateras är det här fristående webbprogrammet.

    ![Bläddra mellanlagring webbprogrammet innan du byter fack](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. Om alla ser bra ut klickar du på den **växla** knappen på din fristående webbprogram för att flytta innehållet till produktionsmiljön. I så fall måste du växla webbappen och databasen i miljöer under varje **växlingen** igen.

    ![Växla förhandsgranskningen ändras för WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > Om ditt scenario behöver endast push-filer (ingen databasuppdateringar), kontrollera **fack inställningen** för alla de Databasrelaterade *appinställningar* och *anslutning strängar inställningar* i den **Webbprograminställningarna** bladet i Azure-portalen innan du gör den **växla**. I det här fallet %{db_name/, DB_HOST, DB_PASSWORD, DB_USER och inställningar för standardanslutning sträng ska inte visas i förhandsgranskningen ändras när du gör en **växla**. Just nu, när du har slutfört den **växla** åtgärd WordPress-webbapp har endast uppdateringar filer.
    >
    >

    Innan du gör en **växla**, här är produktion WordPress-webbapp.
    ![Produktion webbprogrammet innan du byter fack](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

    Efter den **växla** åtgärden temat har uppdaterats på ditt webbprogram för produktion.

    ![Webbprogram i produktion efter växling fack](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. När du behöver återställa går du till produktion web **Appinställningar**, och klicka på den **växlingen** knappen för att växla webbappen och databasen från produktionen till mellanlagringsplatsen. Tänk på att om ändringar i databasen ingår i en **växla** igen nästa gång du distribuerar till ditt fristående webbprogram som du behöver distribuera databasändringar på den aktuella databasen för din fristående webbprogrammet. Den aktuella databasen kanske tidigare produktionsdatabasen eller fas-databasen.

#### <a name="summary"></a>Sammanfattning
Följande är en generaliserad process för alla program som har en databas:

1. Installera programmet på din lokala miljö.
2. Inkludera miljö-specifika konfigurationer (lokala och Azure Web Apps).
3. Konfigurera dina mellanlagring och produktion miljöer för Web Apps.
4. Om du har ett produktionsprogram som redan körs på Azure, synkronisera produktion innehållet (filer/kod och databasen) till lokala och fristående miljöer.
5. Utveckla ditt program på din lokala miljö.
6. Placera ditt webbprogram för produktion under underhåll eller låst läge och synkronisera databasen innehåll från produktionen för mellanlagrings- och dev miljöer.
7. Distribuera till mellanlagringsmiljön och testning.
8. Distribuera till produktionsmiljön.
9. Upprepa steg 4 till 6.

### <a name="umbraco"></a>Umbraco
I det här avsnittet får du lära dig hur Umbraco CMS använder en anpassad modul för att distribuera i miljöer med flera DevOps. Det här exemplet innehåller en annan metod för att hantera flera utvecklingsmiljöer.

[Umbraco CMS](http://umbraco.com/) är en populär .NET CMS-lösning som används av många utvecklare. Det ger den [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul för distribution från utvecklingsmiljö till Förproduktion till produktionsmiljön. Du kan enkelt skapa en lokal utvecklingsmiljö för en Umbraco CMS-webbapp med hjälp av Visual Studio eller WebMatrix.

- [Skapa en Umbraco webbprogram med Visual Studio](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [Skapa en Umbraco webbapp med WebMatrix](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

Komma ihåg att ta bort den `install` mapp under ditt program och aldrig ladda upp det steg eller produktion webbprogram. Den här kursen använder WebMatrix.

#### <a name="set-up-a-staging-environment"></a>Konfigurera en mellanlagringsmiljön
1. Skapa en distributionsplats som tidigare nämnts för webbprogrammet Umbraco CMS, förutsatt att du redan har en Umbraco CMS-webbapp och körs. Om du inte vill skapa du en från Marketplace.
2. Uppdatera anslutningssträngen för din steget distributionsplatsen så att den pekar till den nya **umbraco-steg-db** databas. Ditt webbprogram för produktion (umbraositecms-1) och fristående webbapp (umbracositecms-1-delen) *måste* pekar på olika databaser.

    ![Uppdatera anslutningssträngen för mellanlagring webbprogram med nya tillfälliga databasen](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. Klicka på **hämta Publiceringsinställningar** för distributionsplatsen **steget**. Den här processen hämtar ett publicera-inställningsfilen som lagrar all information som Visual Studio eller WebMatrix kräver för att publicera programmet från lokal utveckling webbapp till Azure webbapp.

    ![Hämta publicera för mellanlagring webbprogram](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. Öppna ditt webbprogram för lokal utveckling i WebMatrix eller Visual Studio. Den här kursen använder WebMatrix. Först måste du importera filen publicera för fristående webbappen.

    ![Importera publiceringsinställningarna för Umbraco med hjälp av Web Matrix](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. Granska ändringar i dialogrutan och distribuera webbappen lokalt till din Azure webbapp *umbracositecms 1 steg*. När du distribuerar filer direkt till webbappen fristående du utelämnar filer i den `~/app_data/TEMP/` mappen eftersom filerna återskapas när steget webbprogrammet startade. Du bör också hoppa över den `~/app_data/umbraco.config` -fil, som också kommer att genereras om.

    ![Granska publicera ändringar i web matris](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. När du har publicerat Umbraco lokala webbappen till mellanlagring webbappen Bläddra till ditt fristående webbprogram och köra några tester för att utesluta eventuella problem.

#### <a name="set-up-the-courier2-deployment-module"></a>Ställa in modulen Courier2 distribution
Med den [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulen, högerklickar du bara på att push-innehåll, formatmallar och utveckling moduler från en fristående webbapp till en webbapp i produktion. Den här processen minskar risken för att dela ditt webbprogram för produktion när du distribuerar en uppdatering.
Köp en licens för Courier2 för den `*.azurewebsites.net` domän och den anpassade domänen (Säg http://abc.com). När du har köpt licensen placera hämtade licensen (. LIC-fil) i den `bin` mapp.

![Släpp licensfil under bin-mappen](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. [Hämta Courier2](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Logga in på ditt steget webbprogram Säg http://umbracocms-site-stage.azurewebsites.net/umbraco klickar du på den **Developer** -menyn och klicka sedan på **paket** > **installationspaket för lokala**.

    ![Umbraco installationspaketet](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. Överför Courier2 paketet med hjälp av installationsprogrammet.

    ![Överföra courier modul-paket](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. Om du vill konfigurera paketet, måste du uppdatera filen courier.config under den **Config** för ditt webbprogram.

    ```xml
    <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
        <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1.azurewebsites.net</url>
          <user>0</user>
          <!--<login>user@email.com</login> -->
          <!-- <password>user_password</password>-->
          <!-- <passwordEncoding>Clear</passwordEncoding>-->
          </repository>
     </repositories>
     ```

4. Under `<repositories>`, ange den produktion plats URL och användare.
    Om du använder Umbraco standardprovider för medlemskap, Lägg sedan till ID: T för Administration av användare i den &lt;användaren&gt; avsnitt.
    Om du använder en anpassad Umbraco medlemskapsprovider använda `<login>`,`<password>` i modulen Courier2 att ansluta till produktionsplatsen.
    Mer information [Läs dokumentationen om modulen Courier2](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).

5. På motsvarande sätt installera modulen Courier2 på webbplatsen produktion och konfigurera den att peka till webbappen steg i dess respektive courier.config-fil som visas här.

    ```xml
     <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
        <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1-stage.azurewebsites.net</url>
          <user>0</user>
          </repository>
     </repositories>
    ```

6. Klicka på den **Courier2** i infopanelen Umbraco CMS web app och klicka sedan på **platser**. Du bör se databasnamnet som anges i `courier.config`. Gör den här processen på din produktion och ett fristående webbprogram.

    ![Visa måldatabasen web app](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. För att distribuera innehåll från webbplatsen mellanlagringsplatsen till produktionsplatsen, gå till **innehåll**, och välj en befintlig sida eller skapa en ny sida. Att välja en befintlig sida från webbappen där titeln på sidan är **komma igång – nya**, och klicka sedan på **spara och publicera**.

    ![Ändra rubriken på sidan och publicera](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. Högerklicka på den aktuella sidan om du vill visa alla alternativ. Klicka på **Courier** att öppna den **distribution** dialogrutan. Klicka på **distribuera** att initiera distributionen.

    ![Courier modulen distribution dialogrutan](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. Granska ändringar och klicka sedan på **Fortsätt**.

    ![Courier modulen distribution dialogrutan Granska ändringar](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    Distributionsloggen visar om distributionen lyckades.

     ![Visa distributionsloggar Courier-modul](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. Bläddra i ditt webbprogram för produktion för att se om ändringarna visas.

     ![Bläddra webbprogrammet för produktion](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Läs dokumentationen för mer information om hur du använder Courier.

#### <a name="how-to-upgrade-the-umbraco-cms-version"></a>Så här uppgraderar du Umbraco CMS-version
Courier kommer inte hjälper dig att uppgradera från en version av Umbraco CMS till en annan. När du uppgraderar en Umbraco CMS-version, måste du kontrollera med din anpassade moduler eller moduler från partners och Umbraco Core-bibliotek. Här är bästa praxis:

* Säkerhetskopiera alltid din webbappen och databasen innan du uppgraderar. På web apps i Azure kan du ställa in automatiska säkerhetskopieringar för dina webbplatser med hjälp av funktionen för säkerhetskopiering och återställa platsen om det behövs med hjälp av funktionen för återställning. Mer information finns i [säkerhetskopiering av ditt webbprogram](web-sites-backup.md) och [återställa ditt webbprogram](web-sites-restore.md).
* Kontrollera om paket från partner är kompatibel med den version som du uppgraderar till. På paketet hämtningssidan, granska projektet kompatibilitet med Umbraco CMS-version.

Mer information om hur du uppgraderar ditt webbprogram lokalt, [finns allmänna uppgradera](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).

Publicera ändringarna till den tillfälliga webbappen när lokal utvecklingsplatsen har uppgraderats. Testa ditt program. Om alla ser bra ut använder den **växlingen** för att byta ut din fristående plats till produktion webbapp. När du använder den **växla** igen, du kan visa ändringarna som kommer att påverkas i ditt webbprogram konfiguration. Detta **växla** åtgärden växlingar webbappar och databaser. Efter den **växla**web produktionsprogrammet peka mot umbraco-steg-db-databasen och fristående webbappen att peka umbraco-produktprenumeration-db-databas.

![Växla förhandsgranskning för att distribuera Umbraco CMS](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Här är fördelarna med att byta både webbappen och databasen:

* Du kan återställa till den tidigare versionen av ditt webbprogram med en annan **växla** om det uppstår några problem med programmet.
* Du måste distribuera filer och databaser från fristående webbappen till produktion webbappen och databasen för en uppgradering. Många saker går fel när du distribuerar filer och databaser. Med hjälp av den **växla** funktionen fack, kan vi minska avbrottstiden vid en uppgradering och minska risken för fel som kan uppstå när du distribuerar ändringar.
* Du kan göra **A / B-testning** med hjälp av den [test i produktion](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funktion.

Det här exemplet visar flexibilitet i plattformen där du kan skapa anpassade moduler som liknar Umbraco Courier modul som ska hantera distribution mellan miljöer.

## <a name="references"></a>Referenser
[Flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md)

[Skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md)

[Blockera Webbåtkomst till icke-produktion distributionsplatser](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
