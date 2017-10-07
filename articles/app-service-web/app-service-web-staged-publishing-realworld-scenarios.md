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
# <a name="use-devops-environments-effectively-for-your-web-apps"></a><span data-ttu-id="138c5-103">Använda DevOps miljöer effektivt för ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="138c5-103">Use DevOps environments effectively for your web apps</span></span>
<span data-ttu-id="138c5-104">Den här artikeln beskrivs hur du tooset och hantera web application-distributioner när flera versioner av programmet finns i olika miljöer, till exempel utveckling, mellanlagring, quality assurance (QA) och produktion.</span><span class="sxs-lookup"><span data-stu-id="138c5-104">This article shows you how tooset up and manage web application deployments when multiple versions of your application are in various environments, such as development, staging, quality assurance (QA), and production.</span></span> <span data-ttu-id="138c5-105">Varje version av programmet kan betraktas som en utvecklingsmiljö för hello specifikt syfte av distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="138c5-105">Each version of your application can be considered as a development environment for hello specific purpose of your deployment process.</span></span> <span data-ttu-id="138c5-106">Utvecklare kan använda hello QA miljö tootest hello kvaliteten på hello program innan de push hello ändringar tooproduction.</span><span class="sxs-lookup"><span data-stu-id="138c5-106">For example, developers can use hello QA environment tootest hello quality of hello application before they push hello changes tooproduction.</span></span>
<span data-ttu-id="138c5-107">Flera utvecklingsmiljöer kan vara en utmaning eftersom du behöver tootrack kod, hantera resurser (beräkning, webbprogram, databas, cache o.s.v.) och distribuera kod mellan miljöer.</span><span class="sxs-lookup"><span data-stu-id="138c5-107">Multiple development environments can be a challenge because you need tootrack code, manage resources (compute, web app, database, cache, etc.), and deploy code across environments.</span></span>

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a><span data-ttu-id="138c5-108">Konfigurera en icke-produktionsmiljö (steg, utveckling, QA)</span><span class="sxs-lookup"><span data-stu-id="138c5-108">Set up a non-production environment (stage, dev, QA)</span></span>
<span data-ttu-id="138c5-109">När ett webbprogram för produktion är igång, är hello nästa steg toocreate en icke-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="138c5-109">After a production web app is up and running, hello next step is toocreate a non-production environment.</span></span> <span data-ttu-id="138c5-110">toouse distributionsplatser, se till att du kör i läget för hello Standard eller Premium Azure App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="138c5-110">toouse deployment slots, make sure that you are running in hello Standard or Premium Azure App Service plan mode.</span></span> <span data-ttu-id="138c5-111">Distributionsplatser är live-webb-appar som har sina egna värdnamn.</span><span class="sxs-lookup"><span data-stu-id="138c5-111">Deployment slots are live web apps that have their own host names.</span></span> <span data-ttu-id="138c5-112">Web app innehåll och konfiguration element kan växlas mellan två distributionsplatser, inklusive hello produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="138c5-112">Web app content and configuration elements can be swapped between two deployment slots, including hello production slot.</span></span> <span data-ttu-id="138c5-113">När du distribuerar distributionsplatsen för tooa ditt program kan få du hello följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="138c5-113">When you deploy your application tooa deployment slot, you get hello following benefits:</span></span>

- <span data-ttu-id="138c5-114">Du kan validera ändringar tooa webbprogram i en distribution mellanlagringsplatsen innan du byter hello app med hello produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="138c5-114">You can validate changes tooa web app in a staging deployment slot before you swap hello app with hello production slot.</span></span>
- <span data-ttu-id="138c5-115">När du distribuerar en tooa webbprogramplats först och växla till produktion, är alla instanser av hello plats varmkörts innan som ska växlas över till produktion.</span><span class="sxs-lookup"><span data-stu-id="138c5-115">When you deploy a web app tooa slot first and swap it into production, all instances of hello slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="138c5-116">Den här processen eliminerar avbrott när du distribuerar ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="138c5-116">This process eliminates downtime when you deploy your web app.</span></span> <span data-ttu-id="138c5-117">hello trafik omdirigering sker sömlös och inga begäranden tas bort på grund av tooswap åtgärder.</span><span class="sxs-lookup"><span data-stu-id="138c5-117">hello traffic redirection is seamless, and no requests are dropped due tooswap operations.</span></span> <span data-ttu-id="138c5-118">tooautomate hela arbetsflödet, konfigurera [automatiskt växla](web-sites-staged-publishing.md#configure-auto-swap) när före växlingen verifiering inte behövs.</span><span class="sxs-lookup"><span data-stu-id="138c5-118">tooautomate this entire workflow, configure [Auto Swap](web-sites-staged-publishing.md#configure-auto-swap) when pre-swap validation is not needed.</span></span>
- <span data-ttu-id="138c5-119">Efter en växling har hello fack som har hello tidigare mellanlagrad webbprogrammet nu hello tidigare produktion webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="138c5-119">After a swap, hello slot that has hello previously staged web app now has hello previous production web app.</span></span> <span data-ttu-id="138c5-120">Om hello växlas över till hello produktionsplatsen ändras inte som väntat, kan du utföra hello samma Växla omedelbart tooget din ”fungerande” web app igen.</span><span class="sxs-lookup"><span data-stu-id="138c5-120">If hello changes swapped into hello production slot are not as you expected, you can perform hello same swap immediately tooget your "last known good" web app back.</span></span>

<span data-ttu-id="138c5-121">tooset upp en mellanlagringsplatsen för distribution finns [skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="138c5-121">tooset up a staging deployment slot, see [Set up staging environments for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="138c5-122">Alla miljöer bör innehålla en egen uppsättning resurser.</span><span class="sxs-lookup"><span data-stu-id="138c5-122">Every environment should include its own set of resources.</span></span> <span data-ttu-id="138c5-123">Till exempel om ditt program använder en databas, ska sedan produktion och mellanlagring webbprogram använda olika databaser.</span><span class="sxs-lookup"><span data-stu-id="138c5-123">For example, if your web app uses a database, then both production and staging web apps should use different databases.</span></span> <span data-ttu-id="138c5-124">Lägga till fristående development miljö resurser, till exempel databasen, lagring och cache tooset utvecklingsmiljön fristående.</span><span class="sxs-lookup"><span data-stu-id="138c5-124">Add staging development environment resources such as database, storage, or cache tooset your staging development environment.</span></span>

## <a name="examples-of-using-multiple-development-environments"></a><span data-ttu-id="138c5-125">Exempel på användning av flera utvecklingsmiljöer</span><span class="sxs-lookup"><span data-stu-id="138c5-125">Examples of using multiple development environments</span></span>
<span data-ttu-id="138c5-126">Alla projekt bör följa källa Kodhantering med minst två miljöer: utveckling och produktion.</span><span class="sxs-lookup"><span data-stu-id="138c5-126">Any project should follow source code management with at least two environments: development and production.</span></span> <span data-ttu-id="138c5-127">Om du använder innehållshanteringssystem (CMSs), ramverk för programmet, etc. kan hello program inte stöder det här scenariot utan anpassning.</span><span class="sxs-lookup"><span data-stu-id="138c5-127">If you use content management systems (CMSs), application frameworks, etc., hello application might not support this scenario without customization.</span></span> <span data-ttu-id="138c5-128">Denna situation är true för några av hello populära ramverk som beskrivs i följande avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="138c5-128">This eventuality is true for some of hello popular frameworks that are discussed in hello following sections.</span></span> <span data-ttu-id="138c5-129">Många frågor kommer toomind när du arbetar med CMS/ramverk, som:</span><span class="sxs-lookup"><span data-stu-id="138c5-129">Lots of questions come toomind when you work with CMS/frameworks, such as:</span></span>

- <span data-ttu-id="138c5-130">Hur du bryter hello innehåll till olika miljöer?</span><span class="sxs-lookup"><span data-stu-id="138c5-130">How do you break hello content out into different environments?</span></span>
- <span data-ttu-id="138c5-131">Vilka filer kan du ändra utan att påverka framework versionuppdateringar?</span><span class="sxs-lookup"><span data-stu-id="138c5-131">What files can you change without affecting framework version updates?</span></span>
- <span data-ttu-id="138c5-132">Hur hanterar du konfigurationer per miljön?</span><span class="sxs-lookup"><span data-stu-id="138c5-132">How do you manage configurations per environment?</span></span>
- <span data-ttu-id="138c5-133">Hur hanterar du versionuppdateringar för moduler, plugin-program och hello centralt ramverk?</span><span class="sxs-lookup"><span data-stu-id="138c5-133">How do you manage version updates for modules, plugins, and hello core framework?</span></span>

<span data-ttu-id="138c5-134">Det finns många sätt tooset in flera miljöer för projektet.</span><span class="sxs-lookup"><span data-stu-id="138c5-134">There are many ways tooset up multiple environments for your project.</span></span> <span data-ttu-id="138c5-135">hello visar följande exempel en metod för varje respektive program.</span><span class="sxs-lookup"><span data-stu-id="138c5-135">hello following examples show one method for each respective application.</span></span>

### <a name="wordpress"></a><span data-ttu-id="138c5-136">WordPress</span><span class="sxs-lookup"><span data-stu-id="138c5-136">WordPress</span></span>
<span data-ttu-id="138c5-137">I det här avsnittet får du lära dig hur tooset upp ett arbetsflöde för distribution med hjälp av platser för WordPress.</span><span class="sxs-lookup"><span data-stu-id="138c5-137">In this section, you will learn how tooset up a deployment workflow by using slots for WordPress.</span></span> <span data-ttu-id="138c5-138">WordPress, stöder som de flesta CMS-lösningar, inte flera utvecklingsmiljöer utan anpassning.</span><span class="sxs-lookup"><span data-stu-id="138c5-138">WordPress, like most CMS solutions, does not support multiple development environments without customization.</span></span> <span data-ttu-id="138c5-139">funktionen för hello Web Apps i Azure App Service har några funktioner som gör det enkelt toostore konfigurationsinställningar utanför din kod.</span><span class="sxs-lookup"><span data-stu-id="138c5-139">hello Web Apps feature of Azure App Service has a few features that make it easy toostore configuration settings outside your code.</span></span>

1. <span data-ttu-id="138c5-140">Innan du skapar en mellanlagringsplatsen konfigurera ditt program kod toosupport miljöer med flera.</span><span class="sxs-lookup"><span data-stu-id="138c5-140">Before you create a staging slot, set up your application code toosupport multiple environments.</span></span> <span data-ttu-id="138c5-141">toosupport miljöer med flera i WordPress, behöver du tooedit `wp-config.php` på din lokala development web app och lägga till hello följande kod hello början av hello-filen.</span><span class="sxs-lookup"><span data-stu-id="138c5-141">toosupport multiple environments in WordPress, you need tooedit `wp-config.php` on your local development web app and add hello following code at hello beginning of hello file.</span></span> <span data-ttu-id="138c5-142">Den här processen kan toopick hello rätt konfigurationen av programmet baserat på valda hello-miljön.</span><span class="sxs-lookup"><span data-stu-id="138c5-142">This process will enable your application toopick hello correct configuration based on hello selected environment.</span></span>

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

2. <span data-ttu-id="138c5-143">Skapa en mapp under web app rot kallas `config`, och Lägg till hello `wp-config.azure.php` och `wp-config.local.php` filer som representerar dina Azure-miljön och den lokala miljön respektive.</span><span class="sxs-lookup"><span data-stu-id="138c5-143">Create a folder under web app root called `config`, and add hello `wp-config.azure.php` and `wp-config.local.php` files, which represent your Azure environment and local environment respectively.</span></span>

3. <span data-ttu-id="138c5-144">Kopiera följande hello i `wp-config.local.php`:</span><span class="sxs-lookup"><span data-stu-id="138c5-144">Copy hello following in `wp-config.local.php`:</span></span>

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

    <span data-ttu-id="138c5-145">Ange hello säkerhetsnycklar enligt beskrivningen i föregående hello-kod kan hjälpa tooprevent ditt webbprogram från som över, så unika värden.</span><span class="sxs-lookup"><span data-stu-id="138c5-145">Setting hello security keys as illustrated in hello previous code can help tooprevent your web app from being hacked, so use unique values.</span></span> <span data-ttu-id="138c5-146">Om du behöver toogenerate hello sträng för säkerhetsnycklar som nämns i hello kod kan du [gå toohello automatisk generator](https://api.wordpress.org/secret-key/1.1/salt) toocreate nytt nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="138c5-146">If you need toogenerate hello string for security keys mentioned in hello code, you can [go toohello automatic generator](https://api.wordpress.org/secret-key/1.1/salt) toocreate new key/value pairs.</span></span>

4. <span data-ttu-id="138c5-147">Kopiera hello följande kod i `wp-config.azure.php`:</span><span class="sxs-lookup"><span data-stu-id="138c5-147">Copy hello following code in `wp-config.azure.php`:</span></span>

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

#### <a name="use-relative-paths"></a><span data-ttu-id="138c5-148">Använda relativa sökvägar</span><span class="sxs-lookup"><span data-stu-id="138c5-148">Use relative paths</span></span>
<span data-ttu-id="138c5-149">En sista sak tooconfigure i hello WordPress-appen är relativa sökvägar.</span><span class="sxs-lookup"><span data-stu-id="138c5-149">One last thing tooconfigure in hello WordPress app is relative paths.</span></span> <span data-ttu-id="138c5-150">WordPress lagrar URL-information i hello-databas.</span><span class="sxs-lookup"><span data-stu-id="138c5-150">WordPress stores URL information in hello database.</span></span> <span data-ttu-id="138c5-151">Den här gör det svårare att flytta innehåll från en miljö tooanother.</span><span class="sxs-lookup"><span data-stu-id="138c5-151">This storage makes moving content from one environment tooanother more difficult.</span></span> <span data-ttu-id="138c5-152">Du måste tooupdate hello databasen varje gång du flyttar från lokala toostage eller steget tooproduction miljöer.</span><span class="sxs-lookup"><span data-stu-id="138c5-152">You need tooupdate hello database every time you move from local toostage or stage tooproduction environments.</span></span> <span data-ttu-id="138c5-153">tooreduce hello risken för problem som kan uppstå när du distribuerar en databas varje gång du distribuerar från en miljö tooanother, Använd hello [rot relativa länkar plugin-programmet](https://wordpress.org/plugins/root-relative-urls/), som du kan installera med hjälp av hello WordPress-administratör instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="138c5-153">tooreduce hello risk of issues that can be caused with deploying a database every time you deploy from one environment tooanother, use hello [Relative Root links plugin](https://wordpress.org/plugins/root-relative-urls/), which you can install by using hello WordPress administrator dashboard.</span></span>

<span data-ttu-id="138c5-154">Lägg till följande poster tooyour hello `wp-config.php` filen innan hello `That's all, stop editing!` kommentar:</span><span class="sxs-lookup"><span data-stu-id="138c5-154">Add hello following entries tooyour `wp-config.php` file before hello `That's all, stop editing!` comment:</span></span>

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

<span data-ttu-id="138c5-155">Aktivera hello plugin-programmet via hello `Plugins` -menyn i instrumentpanelen i WordPress-administratören.</span><span class="sxs-lookup"><span data-stu-id="138c5-155">Activate hello plugin through hello `Plugins` menu in WordPress administrator dashboard.</span></span> <span data-ttu-id="138c5-156">Spara inställningarna webbadress för WordPress-appen.</span><span class="sxs-lookup"><span data-stu-id="138c5-156">Save your permalink settings for WordPress app.</span></span>

#### <a name="hello-final-wp-configphp-file"></a><span data-ttu-id="138c5-157">hello slutliga `wp-config.php` fil</span><span class="sxs-lookup"><span data-stu-id="138c5-157">hello final `wp-config.php` file</span></span>
<span data-ttu-id="138c5-158">Alla viktiga uppdateringar av WordPress påverkar inte din `wp-config.php`, `wp-config.azure.php`, och `wp-config.local.php` filer.</span><span class="sxs-lookup"><span data-stu-id="138c5-158">Any WordPress core updates will not affect your `wp-config.php`, `wp-config.azure.php`, and `wp-config.local.php` files.</span></span> <span data-ttu-id="138c5-159">Här är en slutlig version av hello `wp-config.php` fil:</span><span class="sxs-lookup"><span data-stu-id="138c5-159">Here's a final version of hello `wp-config.php` file:</span></span>

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

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="138c5-160">Konfigurera en mellanlagringsmiljön</span><span class="sxs-lookup"><span data-stu-id="138c5-160">Set up a staging environment</span></span>
1. <span data-ttu-id="138c5-161">Om du redan har en WordPress-webbapp som körs på din Azure-prenumeration kan du logga in toohello [Azure-portalen](http://portal.azure.com), och sedan gå tooyour WordPress-webbapp.</span><span class="sxs-lookup"><span data-stu-id="138c5-161">If you already have a WordPress web app running on your Azure subscription, sign in toohello [Azure portal](http://portal.azure.com), and then go tooyour WordPress web app.</span></span> <span data-ttu-id="138c5-162">Om du inte har en WordPress-webbappen kan skapa du en från hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="138c5-162">If you don't have a WordPress web app, you can create one from hello Azure Marketplace.</span></span> <span data-ttu-id="138c5-163">Det finns fler toolearn [skapa en WordPress-webbapp i Azure App Service](web-sites-php-web-site-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="138c5-163">toolearn more, see [Create a WordPress web app in Azure App Service](web-sites-php-web-site-gallery.md).</span></span>
<span data-ttu-id="138c5-164">Klicka på **inställningar** > **distributionsfack** > **Lägg till** toocreate en distributionsplats med hello namn *steget*.</span><span class="sxs-lookup"><span data-stu-id="138c5-164">Click **Settings** > **Deployment slots** > **Add** toocreate a deployment slot with hello name *stage*.</span></span> <span data-ttu-id="138c5-165">En distributionsplats är ett annat webbprogram som resurser hello samma resurser som hello primära webbprogram som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="138c5-165">A deployment slot is another web application that shares hello same resources as hello primary web app that you created previously.</span></span>

    ![Skapa steget distributionsplats](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. <span data-ttu-id="138c5-167">Lägg till en annan MySQL-databas, säg `wordpress-stage-db`, tooyour resursgrupp, `wordpressapp-group`.</span><span class="sxs-lookup"><span data-stu-id="138c5-167">Add another MySQL database, say `wordpress-stage-db`, tooyour resource group, `wordpressapp-group`.</span></span>

    ![Lägg till MySQL-databas tooresource grupp](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. <span data-ttu-id="138c5-169">Uppdatera hello anslutningssträngar för steget distribution fack toopoint toohello nya databasen, `wordpress-stage-db`.</span><span class="sxs-lookup"><span data-stu-id="138c5-169">Update hello connection strings for your stage deployment slot toopoint toohello new database, `wordpress-stage-db`.</span></span> <span data-ttu-id="138c5-170">Webbappen produktion `wordpressprodapp`, och organisering webbapp `wordpressprodapp-stage`, måste punkt toodifferent databaser.</span><span class="sxs-lookup"><span data-stu-id="138c5-170">Your production web app, `wordpressprodapp`, and staging web app, `wordpressprodapp-stage`, must point toodifferent databases.</span></span>

#### <a name="configure-environment-specific-app-settings"></a><span data-ttu-id="138c5-171">Konfigurera appinställningar för miljö-specifik</span><span class="sxs-lookup"><span data-stu-id="138c5-171">Configure environment-specific app settings</span></span>
<span data-ttu-id="138c5-172">Utvecklare kan lagra nyckel/värde-par för sträng i Azure som en del av hello konfigurationsinformation, kallas **Appinställningar**, som är kopplad till ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="138c5-172">Developers can store key/value string pairs in Azure as part of hello configuration information, called **App Settings**, that's associated with a web app.</span></span> <span data-ttu-id="138c5-173">Vid körning, webbprogram automatiskt hämtar dessa värden och gör dem tillgängliga toocode som körs i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="138c5-173">At runtime, web apps automatically retrieve these values and make them available toocode that's running in your web app.</span></span> <span data-ttu-id="138c5-174">Från ett säkerhetsperspektiv som är en bra sida fördel eftersom känslig information, till exempel databasanslutningssträngar som innehåller lösenord, aldrig visas i klartext i en fil som `wp-config.php`.</span><span class="sxs-lookup"><span data-stu-id="138c5-174">From a security perspective, that is a nice side benefit because sensitive information, such as database connection strings that include passwords, never show up as clear text in a file such as `wp-config.php`.</span></span>

<span data-ttu-id="138c5-175">Den här processen, vilket förklaras i följande stycken hello, är användbar eftersom den innehåller både ändras och ändringar i databasen för hello WordPress-appen:</span><span class="sxs-lookup"><span data-stu-id="138c5-175">This process, which is explained in hello following paragraphs, is useful because it includes both file changes and database changes for hello WordPress app:</span></span>

* <span data-ttu-id="138c5-176">WordPress versionsuppgradering</span><span class="sxs-lookup"><span data-stu-id="138c5-176">WordPress version upgrade</span></span>
* <span data-ttu-id="138c5-177">Lägga till nya eller redigera eller uppgradera plugin-program</span><span class="sxs-lookup"><span data-stu-id="138c5-177">Add new or edit or upgrade plugins</span></span>
* <span data-ttu-id="138c5-178">Lägga till nya eller redigera eller uppgradera teman</span><span class="sxs-lookup"><span data-stu-id="138c5-178">Add new or edit or upgrade themes</span></span>

<span data-ttu-id="138c5-179">Konfigurera appinställningar för:</span><span class="sxs-lookup"><span data-stu-id="138c5-179">Configure app settings for:</span></span>

* <span data-ttu-id="138c5-180">Information om databas</span><span class="sxs-lookup"><span data-stu-id="138c5-180">Database information</span></span>
* <span data-ttu-id="138c5-181">Aktivera/inaktivera WordPress-loggning</span><span class="sxs-lookup"><span data-stu-id="138c5-181">Turning on/off WordPress logging</span></span>
* <span data-ttu-id="138c5-182">Säkerhetsinställningar för WordPress</span><span class="sxs-lookup"><span data-stu-id="138c5-182">WordPress security settings</span></span>

![App-inställningar för Wordpress-webbapp](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

<span data-ttu-id="138c5-184">Kontrollera att du lägger till hello följande app-inställningar för din web app och fas produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="138c5-184">Make sure that you add hello following app settings for your production web app and stage slot.</span></span> <span data-ttu-id="138c5-185">Observera att hello produktion webbapp och fristående webbprogrammet använda olika databaser.</span><span class="sxs-lookup"><span data-stu-id="138c5-185">Note that hello production web app and staging web app use different databases.</span></span>

1. <span data-ttu-id="138c5-186">Rensa hello **fack inställningen** kryssrutan för alla hello inställningar parametrar utom WP_ENV.</span><span class="sxs-lookup"><span data-stu-id="138c5-186">Clear hello **Slot Setting** checkbox for all hello settings parameters except WP_ENV.</span></span> <span data-ttu-id="138c5-187">Den här processen kommer växla hello konfiguration för webbprogram, innehåll och databasen.</span><span class="sxs-lookup"><span data-stu-id="138c5-187">This process will swap hello configuration for your web app, file content, and database.</span></span> <span data-ttu-id="138c5-188">Om **fack inställningen** är markerat hello webbapp app-inställningar och anslutningskonfiguration sträng kommer *inte* flytta mellan miljöer när du gör en **växla** igen.</span><span class="sxs-lookup"><span data-stu-id="138c5-188">If **Slot Setting** is checked, hello web app’s app settings and connection string configuration will *not* move across environments when doing a **Swap** operation.</span></span> <span data-ttu-id="138c5-189">Alla ändringar i databasen som finns bryter inte ditt webbprogram för produktion.</span><span class="sxs-lookup"><span data-stu-id="138c5-189">Any database changes that are present will not break your production web app.</span></span>

2. <span data-ttu-id="138c5-190">Distribuera hello lokal utveckling miljö web app toohello steget webbappen och databasen med WebMatrix eller -verktyg, till exempel FTP, Git eller PhpMyAdmin.</span><span class="sxs-lookup"><span data-stu-id="138c5-190">Deploy hello local development environment web app toohello stage web app and database by using WebMatrix or tools of your choice, such as FTP, Git, or PhpMyAdmin.</span></span>

    ![Web Matrix publicera dialogrutan för WordPress-webbapp](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. <span data-ttu-id="138c5-192">Bläddra och testa din fristående webbapp.</span><span class="sxs-lookup"><span data-stu-id="138c5-192">Browse and test your staging web app.</span></span> <span data-ttu-id="138c5-193">Med tanke på ett scenario där hello tema av hello webbprogram är toobe uppdateras är det här hello mellanlagring webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="138c5-193">Considering a scenario where hello theme of hello web app is toobe updated, here is hello staging web app.</span></span>

    ![Bläddra mellanlagring webbprogrammet innan du byter fack](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. <span data-ttu-id="138c5-195">Om alla ser bra ut klickar du på hello **växla** knappen på din fristående web app toomove produktionsmiljön innehåll toohello.</span><span class="sxs-lookup"><span data-stu-id="138c5-195">If all looks good, click hello **Swap** button on your staging web app toomove your content toohello production environment.</span></span> <span data-ttu-id="138c5-196">I så fall måste du växla hello webbapp och hello databasen mellan miljöer under varje **växlingen** igen.</span><span class="sxs-lookup"><span data-stu-id="138c5-196">In this case, you swap hello web app and hello database across environments during every **Swap** operation.</span></span>

    ![Växla förhandsgranskningen ändras för WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > <span data-ttu-id="138c5-198">Om ditt scenario måste tooonly push-filer (ingen databasuppdateringar), kontrollera **fack inställningen** för alla hello Databasrelaterade *appinställningar* och *anslutning strängar inställningar*i hello **Webbprograminställningarna** bladet inom hello Azure-portalen innan du gör hello **växla**.</span><span class="sxs-lookup"><span data-stu-id="138c5-198">If your scenario needs tooonly push files (no database updates), then check **Slot Setting** for all hello database-related *app settings* and *connection strings settings* in hello **Web App Settings** blade within hello Azure portal before doing hello **Swap**.</span></span> <span data-ttu-id="138c5-199">I det här fallet %{db_name/, DB_HOST, DB_PASSWORD, DB_USER och inställningar för standardanslutning sträng ska inte visas i förhandsgranskningen ändras när du gör en **växla**.</span><span class="sxs-lookup"><span data-stu-id="138c5-199">In this case, DB_NAME, DB_HOST, DB_PASSWORD, DB_USER, and default connection string settings should not show up in preview changes when you do a **Swap**.</span></span> <span data-ttu-id="138c5-200">Just nu, när du har slutfört hello **växla** åtgärden, hello WordPress-webbapp ska ha hello uppdaterar endast filer.</span><span class="sxs-lookup"><span data-stu-id="138c5-200">At this time, when you complete hello **Swap** operation, hello WordPress web app will have hello updates files only.</span></span>
    >
    >

    <span data-ttu-id="138c5-201">Innan du gör en **växla**, här är hello produktion WordPress-webbapp.</span><span class="sxs-lookup"><span data-stu-id="138c5-201">Before doing a **Swap**, here is hello production WordPress web app.</span></span>
    <span data-ttu-id="138c5-202">![Produktion webbprogrammet innan du byter fack](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span><span class="sxs-lookup"><span data-stu-id="138c5-202">![Production web app before swapping slots](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span></span>

    <span data-ttu-id="138c5-203">Efter hello **växla** åtgärden hello tema har uppdaterats på ditt webbprogram för produktion.</span><span class="sxs-lookup"><span data-stu-id="138c5-203">After hello **Swap** operation, hello theme has been updated on your production web app.</span></span>

    ![Webbprogram i produktion efter växling fack](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. <span data-ttu-id="138c5-205">När du behöver tooroll tillbaka går du toohello produktion web **Appinställningar**, och klicka på hello **växla** knappen tooswap hello webbappen och databasen från toostaging produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="138c5-205">When you need tooroll back, you can go toohello production web **App Settings**, and click hello **Swap** button tooswap hello web app and database from production toostaging slot.</span></span> <span data-ttu-id="138c5-206">Tänk på att om ändringar i databasen ingår i en **växla** igen och sedan hello nästa gång du distribuerar tooyour mellanlagring webbapp behöver du toodeploy hello ändringar toohello aktuella databasen för din fristående webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="138c5-206">Remember that if database changes are included with a **Swap** operation, then hello next time you deploy tooyour staging web app, you need toodeploy hello database changes toohello current database for your staging web app.</span></span> <span data-ttu-id="138c5-207">hello-databasen kan vara hello tidigare produktionsdatabasen eller hello databas.</span><span class="sxs-lookup"><span data-stu-id="138c5-207">hello current database might be hello previous production database or hello stage database.</span></span>

#### <a name="summary"></a><span data-ttu-id="138c5-208">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="138c5-208">Summary</span></span>
<span data-ttu-id="138c5-209">Följande är en generaliserad process för alla program som har en databas:</span><span class="sxs-lookup"><span data-stu-id="138c5-209">Following is a generalized process for any application that has a database:</span></span>

1. <span data-ttu-id="138c5-210">Installera hello programmet på din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="138c5-210">Install hello application on your local environment.</span></span>
2. <span data-ttu-id="138c5-211">Inkludera miljö-specifika konfigurationer (lokala och Azure Web Apps).</span><span class="sxs-lookup"><span data-stu-id="138c5-211">Include environment-specific configurations (local and Azure Web Apps).</span></span>
3. <span data-ttu-id="138c5-212">Konfigurera dina mellanlagring och produktion miljöer för Web Apps.</span><span class="sxs-lookup"><span data-stu-id="138c5-212">Set up your staging and production environments for Web Apps.</span></span>
4. <span data-ttu-id="138c5-213">Om du har ett produktionsprogram som redan körs på Azure kan synkronisera din innehåll (filer/kod och databasen) toolocal och organisering produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="138c5-213">If you have a production application already running on Azure, sync your production content (files/code and database) toolocal and staging environments.</span></span>
5. <span data-ttu-id="138c5-214">Utveckla ditt program på din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="138c5-214">Develop your application on your local environment.</span></span>
6. <span data-ttu-id="138c5-215">Placera ditt webbprogram för produktion under underhåll eller låst läge och synkronisera databasen innehåll från toostaging och dev produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="138c5-215">Place your production web app under maintenance or locked mode, and sync database content from production toostaging and dev environments.</span></span>
7. <span data-ttu-id="138c5-216">Distribuera toohello mellanlagring miljö och testning.</span><span class="sxs-lookup"><span data-stu-id="138c5-216">Deploy toohello staging environment and test.</span></span>
8. <span data-ttu-id="138c5-217">Distribuera tooproduction miljö.</span><span class="sxs-lookup"><span data-stu-id="138c5-217">Deploy tooproduction environment.</span></span>
9. <span data-ttu-id="138c5-218">Upprepa steg 4 till 6.</span><span class="sxs-lookup"><span data-stu-id="138c5-218">Repeat steps 4 through 6.</span></span>

### <a name="umbraco"></a><span data-ttu-id="138c5-219">Umbraco</span><span class="sxs-lookup"><span data-stu-id="138c5-219">Umbraco</span></span>
<span data-ttu-id="138c5-220">I det här avsnittet får du lära dig hur hello Umbraco CMS använder en anpassad modul toodeploy över flera DevOps-miljöer.</span><span class="sxs-lookup"><span data-stu-id="138c5-220">In this section, you will learn how hello Umbraco CMS uses a custom module toodeploy across multiple DevOps environments.</span></span> <span data-ttu-id="138c5-221">Det här exemplet innehåller en annan metod toomanaging flera utvecklingsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="138c5-221">This example provides a different approach toomanaging multiple development environments.</span></span>

<span data-ttu-id="138c5-222">[Umbraco CMS](http://umbraco.com/) är en populär .NET CMS-lösning som används av många utvecklare.</span><span class="sxs-lookup"><span data-stu-id="138c5-222">[Umbraco CMS](http://umbraco.com/) is a popular .NET CMS solution that's used by many developers.</span></span> <span data-ttu-id="138c5-223">Det ger hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulen toodeploy från utvecklingsmiljöer toostaging tooproduction.</span><span class="sxs-lookup"><span data-stu-id="138c5-223">It provides hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module toodeploy from development toostaging tooproduction environments.</span></span> <span data-ttu-id="138c5-224">Du kan enkelt skapa en lokal utvecklingsmiljö för en Umbraco CMS-webbapp med hjälp av Visual Studio eller WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="138c5-224">You can easily create a local development environment for an Umbraco CMS web app by using Visual Studio or WebMatrix.</span></span>

- [<span data-ttu-id="138c5-225">Skapa en Umbraco webbprogram med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="138c5-225">Create an Umbraco web app with Visual Studio</span></span>](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [<span data-ttu-id="138c5-226">Skapa en Umbraco webbapp med WebMatrix</span><span class="sxs-lookup"><span data-stu-id="138c5-226">Create an Umbraco web app with WebMatrix</span></span>](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

<span data-ttu-id="138c5-227">Komma ihåg tooremove hello `install` mapp under programmet, och överföra den aldrig toostage eller produktion webbprogram.</span><span class="sxs-lookup"><span data-stu-id="138c5-227">Always remember tooremove hello `install` folder under your application, and never upload it toostage or production web apps.</span></span> <span data-ttu-id="138c5-228">Den här kursen använder WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="138c5-228">This tutorial uses WebMatrix.</span></span>

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="138c5-229">Konfigurera en mellanlagringsmiljön</span><span class="sxs-lookup"><span data-stu-id="138c5-229">Set up a staging environment</span></span>
1. <span data-ttu-id="138c5-230">Skapa en distributionsplats som tidigare nämnts för hello Umbraco CMS-webbprogram, förutsatt att du redan har en Umbraco CMS-webbapp och körs.</span><span class="sxs-lookup"><span data-stu-id="138c5-230">Create a deployment slot as mentioned previously for hello Umbraco CMS web app, assuming you already have an Umbraco CMS web app up and running.</span></span> <span data-ttu-id="138c5-231">Om du inte vill skapa du en från hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="138c5-231">If you do not, you can create one from hello Marketplace.</span></span>
2. <span data-ttu-id="138c5-232">Uppdatera hello anslutningssträngen för din steget distribution fack toopoint toohello nya **umbraco-steg-db** databas.</span><span class="sxs-lookup"><span data-stu-id="138c5-232">Update hello connection string for your stage deployment slot toopoint toohello new **umbraco-stage-db** database.</span></span> <span data-ttu-id="138c5-233">Ditt webbprogram för produktion (umbraositecms-1) och fristående webbapp (umbracositecms-1-delen) *måste* punkt toodifferent databaser.</span><span class="sxs-lookup"><span data-stu-id="138c5-233">Your production web app (umbraositecms-1) and staging web app (umbracositecms-1-stage) *must* point toodifferent databases.</span></span>

    ![Uppdatera anslutningssträngen för mellanlagring webbprogram med nya tillfälliga databasen](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. <span data-ttu-id="138c5-235">Klicka på **hämta Publiceringsinställningar** för hello distributionsplatsen **steget**.</span><span class="sxs-lookup"><span data-stu-id="138c5-235">Click **Get Publish settings** for hello deployment slot **stage**.</span></span> <span data-ttu-id="138c5-236">Den här processen hämtar ett publicera-inställningsfilen som lagrar alla hello information att Visual Studio eller WebMatrix kräver toopublish programmet från hello lokal utveckling web app toohello Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="138c5-236">This process will download a publish settings file that stores all hello information that Visual Studio or WebMatrix requires toopublish your application from hello local development web app toohello Azure web app.</span></span>

    ![Hämta publicera för hello mellanlagring av webbprogram](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. <span data-ttu-id="138c5-238">Öppna ditt webbprogram för lokal utveckling i WebMatrix eller Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="138c5-238">Open your local development web app in WebMatrix or Visual Studio.</span></span> <span data-ttu-id="138c5-239">Den här kursen använder WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="138c5-239">This tutorial uses WebMatrix.</span></span> <span data-ttu-id="138c5-240">Du måste först tooimport hello inställningsfilen för webbappen mellanlagring för publicering.</span><span class="sxs-lookup"><span data-stu-id="138c5-240">First, you need tooimport hello publish settings file for your staging web app.</span></span>

    ![Importera publiceringsinställningarna för Umbraco med hjälp av Web Matrix](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. <span data-ttu-id="138c5-242">Granska ändringar i dialogrutan för hello och distribuera din lokala web app tooyour Azure webbapp *umbracositecms 1 steg*.</span><span class="sxs-lookup"><span data-stu-id="138c5-242">Review changes in hello dialog box, and deploy your local web app tooyour Azure web app, *umbracositecms-1-stage*.</span></span> <span data-ttu-id="138c5-243">När du distribuerar filer direkt tooyour mellanlagring webbapp du utelämnar filer i hello `~/app_data/TEMP/` mappen eftersom filerna återskapas när hello steget webbprogrammet startade.</span><span class="sxs-lookup"><span data-stu-id="138c5-243">When you deploy files directly tooyour staging web app, you will omit files in hello `~/app_data/TEMP/` folder because these files will be regenerated when hello stage web app is first started.</span></span> <span data-ttu-id="138c5-244">Du bör också hoppa över hello `~/app_data/umbraco.config` -fil, som också kommer att genereras om.</span><span class="sxs-lookup"><span data-stu-id="138c5-244">You should also omit hello `~/app_data/umbraco.config` file, which will also be regenerated.</span></span>

    ![Granska publicera ändringar i web matris](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. <span data-ttu-id="138c5-246">När du har publicerat hello Umbraco lokala web app toohello fristående webbprogrammet Bläddra tooyour fristående webbprogram och köra några tester toorule eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="138c5-246">After you successfully publish hello Umbraco local web app toohello staging web app, browse tooyour staging web app, and run a few tests toorule out any issues.</span></span>

#### <a name="set-up-hello-courier2-deployment-module"></a><span data-ttu-id="138c5-247">Ställ in hello Courier2 distribution</span><span class="sxs-lookup"><span data-stu-id="138c5-247">Set up hello Courier2 deployment module</span></span>
<span data-ttu-id="138c5-248">Med hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulen, högerklickar du bara på toopush innehåll, formatmallar och utveckling moduler från en fristående webbapp för web app tooa produktion.</span><span class="sxs-lookup"><span data-stu-id="138c5-248">With hello [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module, you can simply right-click toopush content, style sheets, and development modules from a staging web app tooa production web app.</span></span> <span data-ttu-id="138c5-249">Den här processen minskar hello risken för att dela ditt webbprogram för produktion när du distribuerar en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="138c5-249">This process reduces hello risk of breaking your production web app when you deploy an update.</span></span>
<span data-ttu-id="138c5-250">Köp en licens för Courier2 för hello `*.azurewebsites.net` domän och den anpassade domänen (Säg http://abc.com).</span><span class="sxs-lookup"><span data-stu-id="138c5-250">Purchase a license for Courier2 for hello `*.azurewebsites.net` domain and your custom domain (say http://abc.com).</span></span> <span data-ttu-id="138c5-251">När du har köpt hello licens plats hello hämtas licens (. LIC-fil) i hello `bin` mapp.</span><span class="sxs-lookup"><span data-stu-id="138c5-251">After you purchase hello license, place hello downloaded license (.LIC file) in hello `bin` folder.</span></span>

![Släpp licensfil under bin-mappen](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. <span data-ttu-id="138c5-253">[Hämta hello Courier2](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span><span class="sxs-lookup"><span data-stu-id="138c5-253">[Download hello Courier2 package](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span></span> <span data-ttu-id="138c5-254">Logga in tooyour steget webbapp Säg http://umbracocms-site-stage.azurewebsites.net/umbraco klickar du på hello **Developer** -menyn och klicka sedan på **paket** > **installera lokala paketet**.</span><span class="sxs-lookup"><span data-stu-id="138c5-254">Sign in tooyour stage web app, say http://umbracocms-site-stage.azurewebsites.net/umbraco, click hello **Developer** menu, and then click **Packages** > **Install local package**.</span></span>

    ![Umbraco installationspaketet](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. <span data-ttu-id="138c5-256">Överför hello Courier2 paketet med hjälp av hello installer.</span><span class="sxs-lookup"><span data-stu-id="138c5-256">Upload hello Courier2 package by using hello installer.</span></span>

    ![Överföra courier modul-paket](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. <span data-ttu-id="138c5-258">tooconfigure hello-paket, du behöver tooupdate hello courier.config fil under hello **Config** för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="138c5-258">tooconfigure hello package, you need tooupdate hello courier.config file under hello **Config** folder of your web app.</span></span>

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

4. <span data-ttu-id="138c5-259">Under `<repositories>`, ange hello produktion URL och användaren platsinformation.</span><span class="sxs-lookup"><span data-stu-id="138c5-259">Under `<repositories>`, enter hello production site URL and user information.</span></span>
    <span data-ttu-id="138c5-260">Om du använder hello Umbraco standardprovider för medlemskap, Lägg sedan till hello-ID för hello Administration användare i hello &lt;användaren&gt; avsnitt.</span><span class="sxs-lookup"><span data-stu-id="138c5-260">If you are using hello default Umbraco membership provider, then add hello ID for hello Administration user in hello &lt;user&gt; section.</span></span>
    <span data-ttu-id="138c5-261">Om du använder en anpassad Umbraco medlemskapsprovider använda `<login>`,`<password>` i hello Courier2 modulen tooconnect toohello produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="138c5-261">If you are using a custom Umbraco membership provider, use `<login>`,`<password>` in hello Courier2 module tooconnect toohello production site.</span></span>
    <span data-ttu-id="138c5-262">Mer information [hello i dokumentationen för hello Courier2 modulen](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span><span class="sxs-lookup"><span data-stu-id="138c5-262">For more details, [review hello documentation for hello Courier2 module](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span></span>

5. <span data-ttu-id="138c5-263">På motsvarande sätt installera hello Courier2 modulen på webbplatsen produktion och konfigurera den toopoint toohello steget webbprogram i dess respektive courier.config-fil som visas här.</span><span class="sxs-lookup"><span data-stu-id="138c5-263">Similarly, install hello Courier2 module on your production site, and configure it toopoint toohello stage web app in its respective courier.config file as shown here.</span></span>

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

6. <span data-ttu-id="138c5-264">Klicka på hello **Courier2** i hello Umbraco CMS web appinstrumentpanelen och klicka sedan på **platser**.</span><span class="sxs-lookup"><span data-stu-id="138c5-264">Click hello **Courier2** tab in hello Umbraco CMS web app dashboard, and then click **Locations**.</span></span> <span data-ttu-id="138c5-265">Du bör se hello databasnamn som anges i `courier.config`.</span><span class="sxs-lookup"><span data-stu-id="138c5-265">You should see hello repository name as mentioned in `courier.config`.</span></span> <span data-ttu-id="138c5-266">Gör den här processen på din produktion och ett fristående webbprogram.</span><span class="sxs-lookup"><span data-stu-id="138c5-266">Do this process on both your production and staging web apps.</span></span>

    ![Visa måldatabasen web app](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. <span data-ttu-id="138c5-268">toodeploy innehåll från hello mellanlagring toohello produktion platser, gå för**innehåll**, och välj en befintlig sida eller skapa en ny sida.</span><span class="sxs-lookup"><span data-stu-id="138c5-268">toodeploy content from hello staging site toohello production site, go too**Content**, and select an existing page or create a new page.</span></span> <span data-ttu-id="138c5-269">Att välja en befintlig sida från webbappen där hello rubriken för hello sida är **komma igång – nya**, och klicka sedan på **spara och publicera**.</span><span class="sxs-lookup"><span data-stu-id="138c5-269">I will select an existing page from my web app where hello title of hello page is **Getting Started – new**, and then click **Save and Publish**.</span></span>

    ![Ändra rubriken på sidan och publicera](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. <span data-ttu-id="138c5-271">Högerklicka på hello ändrade sidan tooview alla hello-alternativ.</span><span class="sxs-lookup"><span data-stu-id="138c5-271">Right-click hello modified page tooview all hello options.</span></span> <span data-ttu-id="138c5-272">Klicka på **Courier** tooopen hello **distribution** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="138c5-272">Click **Courier** tooopen hello **Deployment** dialog box.</span></span> <span data-ttu-id="138c5-273">Klicka på **distribuera** tooinitiate distribution.</span><span class="sxs-lookup"><span data-stu-id="138c5-273">Click **Deploy** tooinitiate deployment.</span></span>

    ![Courier modulen distribution dialogrutan](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. <span data-ttu-id="138c5-275">Granska hello ändringar och klicka sedan på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="138c5-275">Review hello changes, and then click **Continue**.</span></span>

    ![Courier modulen distribution dialogrutan Granska ändringar](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    <span data-ttu-id="138c5-277">Hej distributionsloggen visar om hello distributionen lyckades.</span><span class="sxs-lookup"><span data-stu-id="138c5-277">hello deployment log shows if hello deployment was successful.</span></span>

     ![Visa distributionsloggar Courier-modul](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. <span data-ttu-id="138c5-279">Bläddra i produktion web app toosee om hello ändringarna visas.</span><span class="sxs-lookup"><span data-stu-id="138c5-279">Browse your production web app toosee if hello changes are reflected.</span></span>

     ![Bläddra webbprogrammet för produktion](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

<span data-ttu-id="138c5-281">Mer information om hur toouse Courier, granska hello dokumentationen toolearn.</span><span class="sxs-lookup"><span data-stu-id="138c5-281">toolearn more about how toouse Courier, review hello documentation.</span></span>

#### <a name="how-tooupgrade-hello-umbraco-cms-version"></a><span data-ttu-id="138c5-282">Hur tooupgrade hello Umbraco CMS-version</span><span class="sxs-lookup"><span data-stu-id="138c5-282">How tooupgrade hello Umbraco CMS version</span></span>
<span data-ttu-id="138c5-283">Courier kommer inte hjälper dig att uppgradera från en version av Umbraco CMS tooanother.</span><span class="sxs-lookup"><span data-stu-id="138c5-283">Courier will not help you upgrade from one version of Umbraco CMS tooanother.</span></span> <span data-ttu-id="138c5-284">När du uppgraderar en Umbraco CMS-version, måste du kontrollera med din anpassade moduler eller moduler från partner och hello Umbraco Core bibliotek.</span><span class="sxs-lookup"><span data-stu-id="138c5-284">When you upgrade an Umbraco CMS version, you must check for incompatibilities with your custom modules or modules from partners and hello Umbraco Core libraries.</span></span> <span data-ttu-id="138c5-285">Här är bästa praxis:</span><span class="sxs-lookup"><span data-stu-id="138c5-285">Here are best practices:</span></span>

* <span data-ttu-id="138c5-286">Säkerhetskopiera alltid din webbappen och databasen innan du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="138c5-286">Always back up your web app and database before you upgrade.</span></span> <span data-ttu-id="138c5-287">Du kan ställa in automatiska säkerhetskopieringar för dina webbplatser med hjälp av funktionen för säkerhetskopiering av hello och återställa webbplatsen om det behövs med hjälp av funktionen för återställning av hello på web apps i Azure.</span><span class="sxs-lookup"><span data-stu-id="138c5-287">On web apps in Azure, you can set up automatic backups for your websites by using hello backup feature and restore your site if needed by using hello restore feature.</span></span> <span data-ttu-id="138c5-288">Mer information finns i [hur tooback upp ditt webbprogram](web-sites-backup.md) och [hur toorestore webbappen](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="138c5-288">For more details, see [How tooback up your web app](web-sites-backup.md) and [How toorestore your web app](web-sites-restore.md).</span></span>
* <span data-ttu-id="138c5-289">Kontrollera om paket från partner är kompatibel med hello-versionen som du uppgraderar till.</span><span class="sxs-lookup"><span data-stu-id="138c5-289">Check if packages from partners are compatible with hello version you're upgrading to.</span></span> <span data-ttu-id="138c5-290">Hämtningssidan för hello paketet, granska hello projektet kompatibilitet med Umbraco CMS-version.</span><span class="sxs-lookup"><span data-stu-id="138c5-290">On hello package's download page, review hello project compatibility with Umbraco CMS version.</span></span>

<span data-ttu-id="138c5-291">Mer information om hur tooupgrade webbappen lokalt, [finns hello allmän uppgradera vägledning](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span><span class="sxs-lookup"><span data-stu-id="138c5-291">For more details about how tooupgrade your web app locally, [see hello general upgrade guidance](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span></span>

<span data-ttu-id="138c5-292">När lokal utvecklingsplatsen har uppgraderats kan du publicera hello ändringar toohello mellanlagring webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="138c5-292">After your local development site is upgraded, publish hello changes toohello staging web app.</span></span> <span data-ttu-id="138c5-293">Testa ditt program.</span><span class="sxs-lookup"><span data-stu-id="138c5-293">Test your application.</span></span> <span data-ttu-id="138c5-294">Om alla ser bra ut använda hello **växla** knappen tooswap fristående plats toohello produktion webbappen.</span><span class="sxs-lookup"><span data-stu-id="138c5-294">If all looks good, use hello **Swap** button tooswap your staging site toohello production web app.</span></span> <span data-ttu-id="138c5-295">När du använder hello **växla** igen, du kan visa hello ändringar som kommer att påverkas i ditt webbprogram konfiguration.</span><span class="sxs-lookup"><span data-stu-id="138c5-295">When you use hello **Swap** operation, you can view hello changes that will be affected in your web app's configuration.</span></span> <span data-ttu-id="138c5-296">Detta **växla** åtgärden växlingar hello webbappar och databaser.</span><span class="sxs-lookup"><span data-stu-id="138c5-296">This **Swap** operation swaps hello web apps and databases.</span></span> <span data-ttu-id="138c5-297">Efter hello **växla**hello web app kommer punkt toohello umbraco-steg-db produktionsdatabasen och hello tillfälliga web app kommer punkt tooumbraco-produktprenumeration-db-databasen.</span><span class="sxs-lookup"><span data-stu-id="138c5-297">After hello **Swap**, hello production web app will point toohello umbraco-stage-db database, and hello staging web app will point tooumbraco-prod-db database.</span></span>

![Växla förhandsgranskning för att distribuera Umbraco CMS](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

<span data-ttu-id="138c5-299">Här är fördelarna med att byta både hello webbapp och hello databasen:</span><span class="sxs-lookup"><span data-stu-id="138c5-299">Here are advantages of swapping both hello web app and hello database:</span></span>

* <span data-ttu-id="138c5-300">Du kan återställa toohello tidigare version av ditt webbprogram med en annan **växla** om det uppstår några problem med programmet.</span><span class="sxs-lookup"><span data-stu-id="138c5-300">You can roll back toohello previous version of your web app with another **Swap** if there are any application issues.</span></span>
* <span data-ttu-id="138c5-301">Du måste toodeploy filer och databaser från hello mellanlagring web app toohello produktion webbappen och databasen för en uppgradering.</span><span class="sxs-lookup"><span data-stu-id="138c5-301">For an upgrade, you need toodeploy files and databases from hello staging web app toohello production web app and database.</span></span> <span data-ttu-id="138c5-302">Många saker går fel när du distribuerar filer och databaser.</span><span class="sxs-lookup"><span data-stu-id="138c5-302">Many things can go wrong when you deploy files and databases.</span></span> <span data-ttu-id="138c5-303">Med hjälp av hello **växla** funktionen fack, kan vi minska avbrottstiden vid en uppgradering och hello risken för fel som kan uppstå när du distribuerar ändringar.</span><span class="sxs-lookup"><span data-stu-id="138c5-303">By using hello **Swap** feature of slots, we can reduce downtime during an upgrade and reduce hello risk of failures that can occur when you deploy changes.</span></span>
* <span data-ttu-id="138c5-304">Du kan göra **A / B-testning** med hjälp av hello [test i produktion](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funktion.</span><span class="sxs-lookup"><span data-stu-id="138c5-304">You can do **A/B testing** by using hello [Testing in production](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) feature.</span></span>

<span data-ttu-id="138c5-305">Detta exempel visar du hello flexibilitet hello plattform där du kan skapa anpassade moduler liknande tooUmbraco Courier toomanage distribution mellan miljöer.</span><span class="sxs-lookup"><span data-stu-id="138c5-305">This example shows you hello flexibility of hello platform where you can build custom modules similar tooUmbraco Courier module toomanage deployment across environments.</span></span>

## <a name="references"></a><span data-ttu-id="138c5-306">Referenser</span><span class="sxs-lookup"><span data-stu-id="138c5-306">References</span></span>
[<span data-ttu-id="138c5-307">Flexibel programvaruutveckling med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="138c5-307">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)

[<span data-ttu-id="138c5-308">Skapa mellanlagringsmiljöer för web apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="138c5-308">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)

[<span data-ttu-id="138c5-309">Hur tooblock web åt toonon produktion distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="138c5-309">How tooblock web access toonon-production deployment slots</span></span>](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
