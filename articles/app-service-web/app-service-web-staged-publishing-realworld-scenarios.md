---
title: "aaaUse DevOps miljöer effektivt för ditt webbprogram | Microsoft Docs"
description: "Lär dig hur toouse distribution fack tooset in och hantera flera utvecklingsmiljöer för ditt program"
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
ms.openlocfilehash: 61a552e735a4ad9769b661d7c988744074ba2962
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Använda DevOps miljöer effektivt för ditt webbprogram
Den här artikeln beskrivs hur du tooset och hantera web application-distributioner när flera versioner av programmet finns i olika miljöer, till exempel utveckling, mellanlagring, quality assurance (QA) och produktion. Varje version av programmet kan betraktas som en utvecklingsmiljö för hello specifikt syfte av distributionsprocessen. Utvecklare kan använda hello QA miljö tootest hello kvaliteten på hello program innan de push hello ändringar tooproduction.
Flera utvecklingsmiljöer kan vara en utmaning eftersom du behöver tootrack kod, hantera resurser (beräkning, webbprogram, databas, cache o.s.v.) och distribuera kod mellan miljöer.

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a>Konfigurera en icke-produktionsmiljö (steg, utveckling, QA)
När ett webbprogram för produktion är igång, är hello nästa steg toocreate en icke-produktionsmiljö. toouse distributionsplatser, se till att du kör i läget för hello Standard eller Premium Azure App Service-plan. Distributionsplatser är live-webb-appar som har sina egna värdnamn. Web app innehåll och konfiguration element kan växlas mellan två distributionsplatser, inklusive hello produktionsplatsen. När du distribuerar distributionsplatsen för tooa ditt program kan få du hello följande fördelar:

- Du kan validera ändringar tooa webbprogram i en distribution mellanlagringsplatsen innan du byter hello app med hello produktionsplatsen.
- När du distribuerar en tooa webbprogramplats först och växla till produktion, är alla instanser av hello plats varmkörts innan som ska växlas över till produktion. Den här processen eliminerar avbrott när du distribuerar ditt webbprogram. hello trafik omdirigering sker sömlös och inga begäranden tas bort på grund av tooswap åtgärder. tooautomate hela arbetsflödet, konfigurera [automatiskt växla](web-sites-staged-publishing.md#configure-auto-swap) när före växlingen verifiering inte behövs.
- Efter en växling har hello fack som har hello tidigare mellanlagrad webbprogrammet nu hello tidigare produktion webbprogrammet. Om hello växlas över till hello produktionsplatsen ändras inte som väntat, kan du utföra hello samma Växla omedelbart tooget din ”fungerande” web app igen.

tooset upp en mellanlagringsplatsen för distribution finns [skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md). Alla miljöer bör innehålla en egen uppsättning resurser. Till exempel om ditt program använder en databas, ska sedan produktion och mellanlagring webbprogram använda olika databaser. Lägga till fristående development miljö resurser, till exempel databasen, lagring och cache tooset utvecklingsmiljön fristående.

## <a name="examples-of-using-multiple-development-environments"></a>Exempel på användning av flera utvecklingsmiljöer
Alla projekt bör följa källa Kodhantering med minst två miljöer: utveckling och produktion. Om du använder innehållshanteringssystem (CMSs), ramverk för programmet, etc. kan hello program inte stöder det här scenariot utan anpassning. Denna situation är true för några av hello populära ramverk som beskrivs i följande avsnitt hello. Många frågor kommer toomind när du arbetar med CMS/ramverk, som:

- Hur du bryter hello innehåll till olika miljöer?
- Vilka filer kan du ändra utan att påverka framework versionuppdateringar?
- Hur hanterar du konfigurationer per miljön?
- Hur hanterar du versionuppdateringar för moduler, plugin-program och hello centralt ramverk?

Det finns många sätt tooset in flera miljöer för projektet. hello visar följande exempel en metod för varje respektive program.

### <a name="wordpress"></a>WordPress
I det här avsnittet får du lära dig hur tooset upp ett arbetsflöde för distribution med hjälp av platser för WordPress. WordPress, stöder som de flesta CMS-lösningar, inte flera utvecklingsmiljöer utan anpassning. funktionen för hello Web Apps i Azure App Service har några funktioner som gör det enkelt toostore konfigurationsinställningar utanför din kod.

1. Innan du skapar en mellanlagringsplatsen konfigurera ditt program kod toosupport miljöer med flera. toosupport miljöer med flera i WordPress, behöver du tooedit `wp-config.php` på din lokala development web app och lägga till hello följande kod hello början av hello-filen. Den här processen kan toopick hello rätt konfigurationen av programmet baserat på valda hello-miljön.

    ```
    // Support multiple environments
    // set hello config file based on current environment
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
    // include hello config file if it exists, otherwise WP is going toofail
    require_once $path. $config_file;
    ```

2. Skapa en mapp under web app rot kallas `config`, och Lägg till hello `wp-config.azure.php` och `wp-config.local.php` filer som representerar dina Azure-miljön och den lokala miljön respektive.

3. Kopiera följande hello i `wp-config.local.php`:

    ```
    <?php
    // MySQL settings
    /** hello name of hello database for WordPress */

    define('DB_NAME', 'yourdatabasename');

    /** MySQL database username */
    define('DB_USER', 'yourdbuser');

    /** MySQL database password */
    define('DB_PASSWORD', 'yourpassword');

    /** MySQL hostname */
    define('DB_HOST', 'localhost');
    /**
     * For developers: WordPress debugging mode.
     * * Change this tootrue tooenable hello display of notices during development.
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

    Ange hello säkerhetsnycklar enligt beskrivningen i föregående hello-kod kan hjälpa tooprevent ditt webbprogram från som över, så unika värden. Om du behöver toogenerate hello sträng för säkerhetsnycklar som nämns i hello kod kan du [gå toohello automatisk generator](https://api.wordpress.org/secret-key/1.1/salt) toocreate nytt nyckel/värde-par.

4. Kopiera hello följande kod i `wp-config.azure.php`:

    ```    
    <?php
    // MySQL settings
    /** hello name of hello database for WordPress */

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
    * Change this tootrue tooenable hello display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging tooinvestigate issues without displaying tooend user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on hello page */

    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);

    //Security key settings
    /** If you need toogenerate hello string for security keys mentioned above, you can go hello automatic generator toocreate new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
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
En sista sak tooconfigure i hello WordPress-appen är relativa sökvägar. WordPress lagrar URL-information i hello-databas. Den här gör det svårare att flytta innehåll från en miljö tooanother. Du måste tooupdate hello databasen varje gång du flyttar från lokala toostage eller steget tooproduction miljöer. tooreduce hello risken för problem som kan uppstå när du distribuerar en databas varje gång du distribuerar från en miljö tooanother, Använd hello [rot relativa länkar plugin-programmet](https://wordpress.org/plugins/root-relative-urls/), som du kan installera med hjälp av hello WordPress-administratör instrumentpanelen.

Lägg till följande poster tooyour hello `wp-config.php` filen innan hello `That's all, stop editing!` kommentar:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Aktivera hello plugin-programmet via hello `Plugins` -menyn i instrumentpanelen i WordPress-administratören. Spara inställningarna webbadress för WordPress-appen.

#### <a name="hello-final-wp-configphp-file"></a>hello slutliga `wp-config.php` fil
Alla viktiga uppdateringar av WordPress påverkar inte din `wp-config.php`, `wp-config.azure.php`, och `wp-config.local.php` filer. Här är en slutlig version av hello `wp-config.php` fil:

```
<?php
/**
 * hello base configurations of hello WordPress.
 *
 * This file has hello following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get hello MySQL settings from your web host.
 *
 * This file is used by hello wp-config.php creation script during the
 * installation. You don't have toouse hello web web app, you can just copy this file
 * too"wp-config.php" and fill in hello values.
 *
 * @package WordPress
 */

// Support multiple environments
// set hello config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include hello config file if it exists, otherwise WP is going toofail
  require_once $path. $config_file;
}

/** Database Charset toouse in creating database tables. */
define('DB_CHARSET', 'utf8');

/** hello Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path toohello WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Konfigurera en mellanlagringsmiljön
1. Om du redan har en WordPress-webbapp som körs på din Azure-prenumeration kan du logga in toohello [Azure-portalen](http://portal.azure.com), och sedan gå tooyour WordPress-webbapp. Om du inte har en WordPress-webbappen kan skapa du en från hello Azure Marketplace. Det finns fler toolearn [skapa en WordPress-webbapp i Azure App Service](web-sites-php-web-site-gallery.md).
Klicka på **inställningar** > **distributionsfack** > **Lägg till** toocreate en distributionsplats med hello namn *steget*. En distributionsplats är ett annat webbprogram som resurser hello samma resurser som hello primära webbprogram som du skapade tidigare.

    ![Skapa steget distributionsplats](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. Lägg till en annan MySQL-databas, säg `wordpress-stage-db`, tooyour resursgrupp, `wordpressapp-group`.

    ![Lägg till MySQL-databas tooresource grupp](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. Uppdatera hello anslutningssträngar för steget distribution fack toopoint toohello nya databasen, `wordpress-stage-db`. Webbappen produktion `wordpressprodapp`, och organisering webbapp `wordpressprodapp-stage`, måste punkt toodifferent databaser.

#### <a name="configure-environment-specific-app-settings"></a>Konfigurera appinställningar för miljö-specifik
Utvecklare kan lagra nyckel/värde-par för sträng i Azure som en del av hello konfigurationsinformation, kallas **Appinställningar**, som är kopplad till ett webbprogram. Vid körning, webbprogram automatiskt hämtar dessa värden och gör dem tillgängliga toocode som körs i ditt webbprogram. Från ett säkerhetsperspektiv som är en bra sida fördel eftersom känslig information, till exempel databasanslutningssträngar som innehåller lösenord, aldrig visas i klartext i en fil som `wp-config.php`.

Den här processen, vilket förklaras i följande stycken hello, är användbar eftersom den innehåller både ändras och ändringar i databasen för hello WordPress-appen:

* WordPress versionsuppgradering
* Lägga till nya eller redigera eller uppgradera plugin-program
* Lägga till nya eller redigera eller uppgradera teman

Konfigurera appinställningar för:

* Information om databas
* Aktivera/inaktivera WordPress-loggning
* Säkerhetsinställningar för WordPress

![App-inställningar för Wordpress-webbapp](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Kontrollera att du lägger till hello följande app-inställningar för din web app och fas produktionsplatsen. Observera att hello produktion webbapp och fristående webbprogrammet använda olika databaser.

1. Rensa hello **fack inställningen** kryssrutan för alla hello inställningar parametrar utom WP_ENV. Den här processen kommer växla hello konfiguration för webbprogram, innehåll och databasen. Om **fack inställningen** är markerat hello webbapp app-inställningar och anslutningskonfiguration sträng kommer *inte* flytta mellan miljöer när du gör en **växla** igen. Alla ändringar i databasen som finns bryter inte ditt webbprogram för produktion.

2. Distribuera hello lokal utveckling miljö web app toohello steget webbappen och databasen med WebMatrix eller -verktyg, till exempel FTP, Git eller PhpMyAdmin.

    ![Web Matrix publicera dialogrutan för WordPress-webbapp](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. Bläddra och testa din fristående webbapp. Med tanke på ett scenario där hello tema av hello webbprogram är toobe uppdateras är det här hello mellanlagring webbprogrammet.

    ![Bläddra mellanlagring webbprogrammet innan du byter fack](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. Om alla ser bra ut klickar du på hello **växla** knappen på din fristående web app toomove produktionsmiljön innehåll toohello. I så fall måste du växla hello webbapp och hello databasen mellan miljöer under varje **växlingen** igen.

    ![Växla förhandsgranskningen ändras för WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > Om ditt scenario måste tooonly push-filer (ingen databasuppdateringar), kontrollera **fack inställningen** för alla hello Databasrelaterade *appinställningar* och *anslutning strängar inställningar*i hello **Webbprograminställningarna** bladet inom hello Azure-portalen innan du gör hello **växla**. I det här fallet %{db_name/, DB_HOST, DB_PASSWORD, DB_USER och inställningar för standardanslutning sträng ska inte visas i förhandsgranskningen ändras när du gör en **växla**. Just nu, när du har slutfört hello **växla** åtgärden, hello WordPress-webbapp ska ha hello uppdaterar endast filer.
    >
    >

    Innan du gör en **växla**, här är hello produktion WordPress-webbapp.
    ![Produktion webbprogrammet innan du byter fack](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

    Efter hello **växla** åtgärden hello tema har uppdaterats på ditt webbprogram för produktion.

    ![Webbprogram i produktion efter växling fack](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. När du behöver tooroll tillbaka går du toohello produktion web **Appinställningar**, och klicka på hello **växla** knappen tooswap hello webbappen och databasen från toostaging produktionsplatsen. Tänk på att om ändringar i databasen ingår i en **växla** igen och sedan hello nästa gång du distribuerar tooyour mellanlagring webbapp behöver du toodeploy hello ändringar toohello aktuella databasen för din fristående webbprogrammet. hello-databasen kan vara hello tidigare produktionsdatabasen eller hello databas.

#### <a name="summary"></a>Sammanfattning
Följande är en generaliserad process för alla program som har en databas:

1. Installera hello programmet på din lokala miljö.
2. Inkludera miljö-specifika konfigurationer (lokala och Azure Web Apps).
3. Konfigurera dina mellanlagring och produktion miljöer för Web Apps.
4. Om du har ett produktionsprogram som redan körs på Azure kan synkronisera din innehåll (filer/kod och databasen) toolocal och organisering produktionsmiljöer.
5. Utveckla ditt program på din lokala miljö.
6. Placera ditt webbprogram för produktion under underhåll eller låst läge och synkronisera databasen innehåll från toostaging och dev produktionsmiljöer.
7. Distribuera toohello mellanlagring miljö och testning.
8. Distribuera tooproduction miljö.
9. Upprepa steg 4 till 6.

### <a name="umbraco"></a>Umbraco
I det här avsnittet får du lära dig hur hello Umbraco CMS använder en anpassad modul toodeploy över flera DevOps-miljöer. Det här exemplet innehåller en annan metod toomanaging flera utvecklingsmiljöer.

[Umbraco CMS](http://umbraco.com/) är en populär .NET CMS-lösning som används av många utvecklare. Det ger hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulen toodeploy från utvecklingsmiljöer toostaging tooproduction. Du kan enkelt skapa en lokal utvecklingsmiljö för en Umbraco CMS-webbapp med hjälp av Visual Studio eller WebMatrix.

- [Skapa en Umbraco webbprogram med Visual Studio](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [Skapa en Umbraco webbapp med WebMatrix](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

Komma ihåg tooremove hello `install` mapp under programmet, och överföra den aldrig toostage eller produktion webbprogram. Den här kursen använder WebMatrix.

#### <a name="set-up-a-staging-environment"></a>Konfigurera en mellanlagringsmiljön
1. Skapa en distributionsplats som tidigare nämnts för hello Umbraco CMS-webbprogram, förutsatt att du redan har en Umbraco CMS-webbapp och körs. Om du inte vill skapa du en från hello Marketplace.
2. Uppdatera hello anslutningssträngen för din steget distribution fack toopoint toohello nya **umbraco-steg-db** databas. Ditt webbprogram för produktion (umbraositecms-1) och fristående webbapp (umbracositecms-1-delen) *måste* punkt toodifferent databaser.

    ![Uppdatera anslutningssträngen för mellanlagring webbprogram med nya tillfälliga databasen](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. Klicka på **hämta Publiceringsinställningar** för hello distributionsplatsen **steget**. Den här processen hämtar ett publicera-inställningsfilen som lagrar alla hello information att Visual Studio eller WebMatrix kräver toopublish programmet från hello lokal utveckling web app toohello Azure webbapp.

    ![Hämta publicera för hello mellanlagring av webbprogram](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. Öppna ditt webbprogram för lokal utveckling i WebMatrix eller Visual Studio. Den här kursen använder WebMatrix. Du måste först tooimport hello inställningsfilen för webbappen mellanlagring för publicering.

    ![Importera publiceringsinställningarna för Umbraco med hjälp av Web Matrix](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. Granska ändringar i dialogrutan för hello och distribuera din lokala web app tooyour Azure webbapp *umbracositecms 1 steg*. När du distribuerar filer direkt tooyour mellanlagring webbapp du utelämnar filer i hello `~/app_data/TEMP/` mappen eftersom filerna återskapas när hello steget webbprogrammet startade. Du bör också hoppa över hello `~/app_data/umbraco.config` -fil, som också kommer att genereras om.

    ![Granska publicera ändringar i web matris](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. När du har publicerat hello Umbraco lokala web app toohello fristående webbprogrammet Bläddra tooyour fristående webbprogram och köra några tester toorule eventuella problem.

#### <a name="set-up-hello-courier2-deployment-module"></a>Ställ in hello Courier2 distribution
Med hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulen, högerklickar du bara på toopush innehåll, formatmallar och utveckling moduler från en fristående webbapp för web app tooa produktion. Den här processen minskar hello risken för att dela ditt webbprogram för produktion när du distribuerar en uppdatering.
Köp en licens för Courier2 för hello `*.azurewebsites.net` domän och den anpassade domänen (Säg http://abc.com). När du har köpt hello licens plats hello hämtas licens (. LIC-fil) i hello `bin` mapp.

![Släpp licensfil under bin-mappen](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. [Hämta hello Courier2](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Logga in tooyour steget webbapp Säg http://umbracocms-site-stage.azurewebsites.net/umbraco klickar du på hello **Developer** -menyn och klicka sedan på **paket** > **installera lokala paketet**.

    ![Umbraco installationspaketet](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. Överför hello Courier2 paketet med hjälp av hello installer.

    ![Överföra courier modul-paket](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. tooconfigure hello-paket, du behöver tooupdate hello courier.config fil under hello **Config** för ditt webbprogram.

    ```xml
    <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how tooconnect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set hello passwordEncoding tooclear: -->
        <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1.azurewebsites.net</url>
          <user>0</user>
          <!--<login>user@email.com</login> -->
          <!-- <password>user_password</password>-->
          <!-- <passwordEncoding>Clear</passwordEncoding>-->
          </repository>
     </repositories>
     ```

4. Under `<repositories>`, ange hello produktion URL och användaren platsinformation.
    Om du använder hello Umbraco standardprovider för medlemskap, Lägg sedan till hello-ID för hello Administration användare i hello &lt;användaren&gt; avsnitt.
    Om du använder en anpassad Umbraco medlemskapsprovider använda `<login>`,`<password>` i hello Courier2 modulen tooconnect toohello produktionsplatsen.
    Mer information [hello i dokumentationen för hello Courier2 modulen](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).

5. På motsvarande sätt installera hello Courier2 modulen på webbplatsen produktion och konfigurera den toopoint toohello steget webbprogram i dess respektive courier.config-fil som visas här.

    ```xml
     <!-- Repository connection settings -->
     <!-- For each site, a custom repository must be configured, so Courier knows how tooconnect and authenticate-->
     <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set hello passwordEncoding tooclear: -->
        <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
          <url>http://umbracositecms-1-stage.azurewebsites.net</url>
          <user>0</user>
          </repository>
     </repositories>
    ```

6. Klicka på hello **Courier2** i hello Umbraco CMS web appinstrumentpanelen och klicka sedan på **platser**. Du bör se hello databasnamn som anges i `courier.config`. Gör den här processen på din produktion och ett fristående webbprogram.

    ![Visa måldatabasen web app](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. toodeploy innehåll från hello mellanlagring toohello produktion platser, gå för**innehåll**, och välj en befintlig sida eller skapa en ny sida. Att välja en befintlig sida från webbappen där hello rubriken för hello sida är **komma igång – nya**, och klicka sedan på **spara och publicera**.

    ![Ändra rubriken på sidan och publicera](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. Högerklicka på hello ändrade sidan tooview alla hello-alternativ. Klicka på **Courier** tooopen hello **distribution** dialogrutan. Klicka på **distribuera** tooinitiate distribution.

    ![Courier modulen distribution dialogrutan](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. Granska hello ändringar och klicka sedan på **Fortsätt**.

    ![Courier modulen distribution dialogrutan Granska ändringar](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    Hej distributionsloggen visar om hello distributionen lyckades.

     ![Visa distributionsloggar Courier-modul](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. Bläddra i produktion web app toosee om hello ändringarna visas.

     ![Bläddra webbprogrammet för produktion](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Mer information om hur toouse Courier, granska hello dokumentationen toolearn.

#### <a name="how-tooupgrade-hello-umbraco-cms-version"></a>Hur tooupgrade hello Umbraco CMS-version
Courier kommer inte hjälper dig att uppgradera från en version av Umbraco CMS tooanother. När du uppgraderar en Umbraco CMS-version, måste du kontrollera med din anpassade moduler eller moduler från partner och hello Umbraco Core bibliotek. Här är bästa praxis:

* Säkerhetskopiera alltid din webbappen och databasen innan du uppgraderar. Du kan ställa in automatiska säkerhetskopieringar för dina webbplatser med hjälp av funktionen för säkerhetskopiering av hello och återställa webbplatsen om det behövs med hjälp av funktionen för återställning av hello på web apps i Azure. Mer information finns i [hur tooback upp ditt webbprogram](web-sites-backup.md) och [hur toorestore webbappen](web-sites-restore.md).
* Kontrollera om paket från partner är kompatibel med hello-versionen som du uppgraderar till. Hämtningssidan för hello paketet, granska hello projektet kompatibilitet med Umbraco CMS-version.

Mer information om hur tooupgrade webbappen lokalt, [finns hello allmän uppgradera vägledning](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).

När lokal utvecklingsplatsen har uppgraderats kan du publicera hello ändringar toohello mellanlagring webbprogrammet. Testa ditt program. Om alla ser bra ut använda hello **växla** knappen tooswap fristående plats toohello produktion webbappen. När du använder hello **växla** igen, du kan visa hello ändringar som kommer att påverkas i ditt webbprogram konfiguration. Detta **växla** åtgärden växlingar hello webbappar och databaser. Efter hello **växla**hello web app kommer punkt toohello umbraco-steg-db produktionsdatabasen och hello tillfälliga web app kommer punkt tooumbraco-produktprenumeration-db-databasen.

![Växla förhandsgranskning för att distribuera Umbraco CMS](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Här är fördelarna med att byta både hello webbapp och hello databasen:

* Du kan återställa toohello tidigare version av ditt webbprogram med en annan **växla** om det uppstår några problem med programmet.
* Du måste toodeploy filer och databaser från hello mellanlagring web app toohello produktion webbappen och databasen för en uppgradering. Många saker går fel när du distribuerar filer och databaser. Med hjälp av hello **växla** funktionen fack, kan vi minska avbrottstiden vid en uppgradering och hello risken för fel som kan uppstå när du distribuerar ändringar.
* Du kan göra **A / B-testning** med hjälp av hello [test i produktion](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funktion.

Detta exempel visar du hello flexibilitet hello plattform där du kan skapa anpassade moduler liknande tooUmbraco Courier toomanage distribution mellan miljöer.

## <a name="references"></a>Referenser
[Flexibel programvaruutveckling med Azure App Service](app-service-agile-software-development.md)

[Skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md)

[Hur tooblock web åt toonon produktion distributionsplatser](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
