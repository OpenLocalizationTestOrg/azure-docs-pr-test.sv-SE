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
# <a name="use-devops-environments-effectively-for-your-web-apps"></a><span data-ttu-id="9fd8f-103">Använda DevOps miljöer effektivt för ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="9fd8f-103">Use DevOps environments effectively for your web apps</span></span>
<span data-ttu-id="9fd8f-104">Den här artikeln visar hur du ställer in och hantera web application-distributioner om flera versioner av programmet finns i olika miljöer, till exempel utveckling, mellanlagring, quality assurance (QA) och produktion.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-104">This article shows you how to set up and manage web application deployments when multiple versions of your application are in various environments, such as development, staging, quality assurance (QA), and production.</span></span> <span data-ttu-id="9fd8f-105">Varje version av programmet kan betraktas som en utvecklingsmiljö syfte av distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-105">Each version of your application can be considered as a development environment for the specific purpose of your deployment process.</span></span> <span data-ttu-id="9fd8f-106">Utvecklare kan använda QA-miljö för att testa kvaliteten på programmet innan de genomför ändringarna i produktion.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-106">For example, developers can use the QA environment to test the quality of the application before they push the changes to production.</span></span>
<span data-ttu-id="9fd8f-107">Flera utvecklingsmiljöer kan vara en utmaning eftersom du behöver spåra kod, hantera resurser (beräkning, webbprogram, databas, cache o.s.v.) och distribuera kod mellan miljöer.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-107">Multiple development environments can be a challenge because you need to track code, manage resources (compute, web app, database, cache, etc.), and deploy code across environments.</span></span>

## <a name="set-up-a-non-production-environment-stage-dev-qa"></a><span data-ttu-id="9fd8f-108">Konfigurera en icke-produktionsmiljö (steg, utveckling, QA)</span><span class="sxs-lookup"><span data-stu-id="9fd8f-108">Set up a non-production environment (stage, dev, QA)</span></span>
<span data-ttu-id="9fd8f-109">När ett webbprogram för produktion är igång, är nästa steg att skapa en icke-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-109">After a production web app is up and running, the next step is to create a non-production environment.</span></span> <span data-ttu-id="9fd8f-110">Kontrollera att du kör i läget Standard eller Premium Azure App Service-plan för att använda distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-110">To use deployment slots, make sure that you are running in the Standard or Premium Azure App Service plan mode.</span></span> <span data-ttu-id="9fd8f-111">Distributionsplatser är live-webb-appar som har sina egna värdnamn.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-111">Deployment slots are live web apps that have their own host names.</span></span> <span data-ttu-id="9fd8f-112">Web app innehåll och konfiguration element kan växlas mellan två distributionsplatser, inklusive produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-112">Web app content and configuration elements can be swapped between two deployment slots, including the production slot.</span></span> <span data-ttu-id="9fd8f-113">När du distribuerar ditt program till en distributionsplats kan få du följande fördelar:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-113">When you deploy your application to a deployment slot, you get the following benefits:</span></span>

- <span data-ttu-id="9fd8f-114">Du kan validera ändringar av en webbapp i en distribution mellanlagringsplatsen innan du byter appen med produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-114">You can validate changes to a web app in a staging deployment slot before you swap the app with the production slot.</span></span>
- <span data-ttu-id="9fd8f-115">När du först distribuera en webbapp till en plats och växla till produktion, är alla instanser av plats varmkörts innan som ska växlas över till produktion.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-115">When you deploy a web app to a slot first and swap it into production, all instances of the slot are warmed up before being swapped into production.</span></span> <span data-ttu-id="9fd8f-116">Den här processen eliminerar avbrott när du distribuerar ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-116">This process eliminates downtime when you deploy your web app.</span></span> <span data-ttu-id="9fd8f-117">Trafik för omdirigering är sömlös och inga begäranden tas bort på grund av byte åtgärder.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-117">The traffic redirection is seamless, and no requests are dropped due to swap operations.</span></span> <span data-ttu-id="9fd8f-118">Om du vill automatisera hela arbetsflödet, konfigurera [automatiskt växla](web-sites-staged-publishing.md#configure-auto-swap) när före växlingen verifiering inte behövs.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-118">To automate this entire workflow, configure [Auto Swap](web-sites-staged-publishing.md#configure-auto-swap) when pre-swap validation is not needed.</span></span>
- <span data-ttu-id="9fd8f-119">Efter en växling har det fack som har tidigare mellanlagrade webbprogrammet nu tidigare webbprogrammet för produktion.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-119">After a swap, the slot that has the previously staged web app now has the previous production web app.</span></span> <span data-ttu-id="9fd8f-120">Om ändringarna växlas över till produktionsplatsen är inte som du förväntade dig, kan du utföra samma växlingen direkt för att få din ”senast fungerande konfiguration” web app igen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-120">If the changes swapped into the production slot are not as you expected, you can perform the same swap immediately to get your "last known good" web app back.</span></span>

<span data-ttu-id="9fd8f-121">Om du vill konfigurera en distribution mellanlagringsplatsen finns [skapa mellanlagringsmiljöer för web apps i Azure App Service](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="9fd8f-121">To set up a staging deployment slot, see [Set up staging environments for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="9fd8f-122">Alla miljöer bör innehålla en egen uppsättning resurser.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-122">Every environment should include its own set of resources.</span></span> <span data-ttu-id="9fd8f-123">Till exempel om ditt program använder en databas, ska sedan produktion och mellanlagring webbprogram använda olika databaser.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-123">For example, if your web app uses a database, then both production and staging web apps should use different databases.</span></span> <span data-ttu-id="9fd8f-124">Lägga till fristående development miljö resurser som databasen, lagring eller cacheminnet för att ange din fristående utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-124">Add staging development environment resources such as database, storage, or cache to set your staging development environment.</span></span>

## <a name="examples-of-using-multiple-development-environments"></a><span data-ttu-id="9fd8f-125">Exempel på användning av flera utvecklingsmiljöer</span><span class="sxs-lookup"><span data-stu-id="9fd8f-125">Examples of using multiple development environments</span></span>
<span data-ttu-id="9fd8f-126">Alla projekt bör följa källa Kodhantering med minst två miljöer: utveckling och produktion.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-126">Any project should follow source code management with at least two environments: development and production.</span></span> <span data-ttu-id="9fd8f-127">Om du använder innehållshanteringssystem (CMSs), ramverk för programmet, etc., kan programmet inte stöd för det här scenariot utan anpassning.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-127">If you use content management systems (CMSs), application frameworks, etc., the application might not support this scenario without customization.</span></span> <span data-ttu-id="9fd8f-128">Denna situation är true för några av populära ramverk som beskrivs i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-128">This eventuality is true for some of the popular frameworks that are discussed in the following sections.</span></span> <span data-ttu-id="9fd8f-129">Många frågor kommer att tänka på när du arbetar med CMS/ramverk, som:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-129">Lots of questions come to mind when you work with CMS/frameworks, such as:</span></span>

- <span data-ttu-id="9fd8f-130">Hur du bryter innehållet till olika miljöer?</span><span class="sxs-lookup"><span data-stu-id="9fd8f-130">How do you break the content out into different environments?</span></span>
- <span data-ttu-id="9fd8f-131">Vilka filer kan du ändra utan att påverka framework versionuppdateringar?</span><span class="sxs-lookup"><span data-stu-id="9fd8f-131">What files can you change without affecting framework version updates?</span></span>
- <span data-ttu-id="9fd8f-132">Hur hanterar du konfigurationer per miljön?</span><span class="sxs-lookup"><span data-stu-id="9fd8f-132">How do you manage configurations per environment?</span></span>
- <span data-ttu-id="9fd8f-133">Hur hanterar du versionuppdateringar för moduler, plugin-program och centralt ramverk?</span><span class="sxs-lookup"><span data-stu-id="9fd8f-133">How do you manage version updates for modules, plugins, and the core framework?</span></span>

<span data-ttu-id="9fd8f-134">Det finns många sätt att konfigurera flera miljöer för projektet.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-134">There are many ways to set up multiple environments for your project.</span></span> <span data-ttu-id="9fd8f-135">I följande exempel visas en metod för varje respektive program.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-135">The following examples show one method for each respective application.</span></span>

### <a name="wordpress"></a><span data-ttu-id="9fd8f-136">WordPress</span><span class="sxs-lookup"><span data-stu-id="9fd8f-136">WordPress</span></span>
<span data-ttu-id="9fd8f-137">I det här avsnittet får du lära dig hur du ställer in ett arbetsflöde för distribution med platser för WordPress.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-137">In this section, you will learn how to set up a deployment workflow by using slots for WordPress.</span></span> <span data-ttu-id="9fd8f-138">WordPress, stöder som de flesta CMS-lösningar, inte flera utvecklingsmiljöer utan anpassning.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-138">WordPress, like most CMS solutions, does not support multiple development environments without customization.</span></span> <span data-ttu-id="9fd8f-139">Funktionen Web Apps i Azure App Service har några funktioner som gör det enkelt att spara konfigurationsinställningarna utanför din kod.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-139">The Web Apps feature of Azure App Service has a few features that make it easy to store configuration settings outside your code.</span></span>

1. <span data-ttu-id="9fd8f-140">Innan du skapar en mellanlagringsplatsen kan du ställa in din programkod för att stödja flera miljöer.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-140">Before you create a staging slot, set up your application code to support multiple environments.</span></span> <span data-ttu-id="9fd8f-141">För att stödja flera miljöer i WordPress, måste du redigera `wp-config.php` på din lokala development web app och Lägg till följande kod i början av filen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-141">To support multiple environments in WordPress, you need to edit `wp-config.php` on your local development web app and add the following code at the beginning of the file.</span></span> <span data-ttu-id="9fd8f-142">Den här processen kan ditt program att välja rätt konfiguration baserat på den valda miljön.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-142">This process will enable your application to pick the correct configuration based on the selected environment.</span></span>

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

2. <span data-ttu-id="9fd8f-143">Skapa en mapp under web app rot kallas `config`, och Lägg till den `wp-config.azure.php` och `wp-config.local.php` filer som representerar dina Azure-miljön och den lokala miljön respektive.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-143">Create a folder under web app root called `config`, and add the `wp-config.azure.php` and `wp-config.local.php` files, which represent your Azure environment and local environment respectively.</span></span>

3. <span data-ttu-id="9fd8f-144">Kopiera följande i `wp-config.local.php`:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-144">Copy the following in `wp-config.local.php`:</span></span>

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

    <span data-ttu-id="9fd8f-145">Inställningen säkerheten nycklar enligt beskrivningen i föregående kod kan hjälpa för att förhindra att ditt webbprogram som över, så Använd unika värden.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-145">Setting the security keys as illustrated in the previous code can help to prevent your web app from being hacked, so use unique values.</span></span> <span data-ttu-id="9fd8f-146">Om du behöver skapa en sträng för säkerhetsnycklar anges i koden, kan du [gå till automatisk generator](https://api.wordpress.org/secret-key/1.1/salt) att skapa ny nyckel/värde-par.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-146">If you need to generate the string for security keys mentioned in the code, you can [go to the automatic generator](https://api.wordpress.org/secret-key/1.1/salt) to create new key/value pairs.</span></span>

4. <span data-ttu-id="9fd8f-147">Kopiera följande kod i `wp-config.azure.php`:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-147">Copy the following code in `wp-config.azure.php`:</span></span>

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

#### <a name="use-relative-paths"></a><span data-ttu-id="9fd8f-148">Använda relativa sökvägar</span><span class="sxs-lookup"><span data-stu-id="9fd8f-148">Use relative paths</span></span>
<span data-ttu-id="9fd8f-149">En sista åtgärd för att konfigurera i WordPress-appen är relativa sökvägar.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-149">One last thing to configure in the WordPress app is relative paths.</span></span> <span data-ttu-id="9fd8f-150">WordPress lagrar URL-information i databasen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-150">WordPress stores URL information in the database.</span></span> <span data-ttu-id="9fd8f-151">Den här lagringsmedia kan flytta innehåll från en miljö till en annan svårare.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-151">This storage makes moving content from one environment to another more difficult.</span></span> <span data-ttu-id="9fd8f-152">Du måste uppdatera databasen för varje gång du flyttar från lokal till steget eller mellanlagra till produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-152">You need to update the database every time you move from local to stage or stage to production environments.</span></span> <span data-ttu-id="9fd8f-153">Om du vill minska risken för problem som kan uppstå när du distribuerar en databas varje gång du distribuerar från en miljö till en annan, använder den [rot relativa länkar plugin-programmet](https://wordpress.org/plugins/root-relative-urls/), som du kan installera med hjälp av instrumentpanelen för WordPress-administratör.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-153">To reduce the risk of issues that can be caused with deploying a database every time you deploy from one environment to another, use the [Relative Root links plugin](https://wordpress.org/plugins/root-relative-urls/), which you can install by using the WordPress administrator dashboard.</span></span>

<span data-ttu-id="9fd8f-154">Lägga till följande poster i din `wp-config.php` filen innan den `That's all, stop editing!` kommentar:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-154">Add the following entries to your `wp-config.php` file before the `That's all, stop editing!` comment:</span></span>

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

<span data-ttu-id="9fd8f-155">Aktivera plugin-programmet via den `Plugins` -menyn i instrumentpanelen i WordPress-administratören.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-155">Activate the plugin through the `Plugins` menu in WordPress administrator dashboard.</span></span> <span data-ttu-id="9fd8f-156">Spara inställningarna webbadress för WordPress-appen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-156">Save your permalink settings for WordPress app.</span></span>

#### <a name="the-final-wp-configphp-file"></a><span data-ttu-id="9fd8f-157">Slutliga `wp-config.php` fil</span><span class="sxs-lookup"><span data-stu-id="9fd8f-157">The final `wp-config.php` file</span></span>
<span data-ttu-id="9fd8f-158">Alla viktiga uppdateringar av WordPress påverkar inte din `wp-config.php`, `wp-config.azure.php`, och `wp-config.local.php` filer.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-158">Any WordPress core updates will not affect your `wp-config.php`, `wp-config.azure.php`, and `wp-config.local.php` files.</span></span> <span data-ttu-id="9fd8f-159">Här är en slutlig version av den `wp-config.php` filen:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-159">Here's a final version of the `wp-config.php` file:</span></span>

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

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="9fd8f-160">Konfigurera en mellanlagringsmiljön</span><span class="sxs-lookup"><span data-stu-id="9fd8f-160">Set up a staging environment</span></span>
1. <span data-ttu-id="9fd8f-161">Om du redan har en WordPress-webbapp som körs på Azure-prenumerationen, logga in på den [Azure-portalen](http://portal.azure.com), och gå sedan till din WordPress-webbapp.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-161">If you already have a WordPress web app running on your Azure subscription, sign in to the [Azure portal](http://portal.azure.com), and then go to your WordPress web app.</span></span> <span data-ttu-id="9fd8f-162">Om du inte har en WordPress-webbappen kan skapa du en från Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-162">If you don't have a WordPress web app, you can create one from the Azure Marketplace.</span></span> <span data-ttu-id="9fd8f-163">Läs mer i [skapa en WordPress-webbapp i Azure App Service](web-sites-php-web-site-gallery.md).</span><span class="sxs-lookup"><span data-stu-id="9fd8f-163">To learn more, see [Create a WordPress web app in Azure App Service](web-sites-php-web-site-gallery.md).</span></span>
<span data-ttu-id="9fd8f-164">Klicka på **inställningar** > **distributionsfack** > **Lägg till** att skapa en distributionsplats med namnet *steget*.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-164">Click **Settings** > **Deployment slots** > **Add** to create a deployment slot with the name *stage*.</span></span> <span data-ttu-id="9fd8f-165">En distributionsplats är ett annat webbprogram som delar samma resurser som den primära webbapp som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-165">A deployment slot is another web application that shares the same resources as the primary web app that you created previously.</span></span>

    ![Skapa steget distributionsplats](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

2. <span data-ttu-id="9fd8f-167">Lägg till en annan MySQL-databas, säg `wordpress-stage-db`, till en resursgrupp, `wordpressapp-group`.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-167">Add another MySQL database, say `wordpress-stage-db`, to your resource group, `wordpressapp-group`.</span></span>

    ![Lägga till MySQL-databas i resursgrupp](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

3. <span data-ttu-id="9fd8f-169">Uppdatera anslutningssträngarna för din steget distributionsplatsen så att den pekar till den nya databasen `wordpress-stage-db`.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-169">Update the connection strings for your stage deployment slot to point to the new database, `wordpress-stage-db`.</span></span> <span data-ttu-id="9fd8f-170">Webbappen produktion `wordpressprodapp`, och organisering webbapp `wordpressprodapp-stage`, måste peka på olika databaser.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-170">Your production web app, `wordpressprodapp`, and staging web app, `wordpressprodapp-stage`, must point to different databases.</span></span>

#### <a name="configure-environment-specific-app-settings"></a><span data-ttu-id="9fd8f-171">Konfigurera appinställningar för miljö-specifik</span><span class="sxs-lookup"><span data-stu-id="9fd8f-171">Configure environment-specific app settings</span></span>
<span data-ttu-id="9fd8f-172">Utvecklare kan lagra nyckel/värde-par för sträng i Azure som en del av den konfigurationsinformation som kallas **Appinställningar**, som är kopplad till ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-172">Developers can store key/value string pairs in Azure as part of the configuration information, called **App Settings**, that's associated with a web app.</span></span> <span data-ttu-id="9fd8f-173">Vid körning, webbprogram automatiskt hämtar dessa värden och göra dem tillgängliga för kod som körs i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-173">At runtime, web apps automatically retrieve these values and make them available to code that's running in your web app.</span></span> <span data-ttu-id="9fd8f-174">Från ett säkerhetsperspektiv som är en bra sida fördel eftersom känslig information, till exempel databasanslutningssträngar som innehåller lösenord, aldrig visas i klartext i en fil som `wp-config.php`.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-174">From a security perspective, that is a nice side benefit because sensitive information, such as database connection strings that include passwords, never show up as clear text in a file such as `wp-config.php`.</span></span>

<span data-ttu-id="9fd8f-175">Den här processen som beskrivs i följande stycken är användbar eftersom den innehåller både ändras och ändringar i databasen för WordPress-appen:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-175">This process, which is explained in the following paragraphs, is useful because it includes both file changes and database changes for the WordPress app:</span></span>

* <span data-ttu-id="9fd8f-176">WordPress versionsuppgradering</span><span class="sxs-lookup"><span data-stu-id="9fd8f-176">WordPress version upgrade</span></span>
* <span data-ttu-id="9fd8f-177">Lägga till nya eller redigera eller uppgradera plugin-program</span><span class="sxs-lookup"><span data-stu-id="9fd8f-177">Add new or edit or upgrade plugins</span></span>
* <span data-ttu-id="9fd8f-178">Lägga till nya eller redigera eller uppgradera teman</span><span class="sxs-lookup"><span data-stu-id="9fd8f-178">Add new or edit or upgrade themes</span></span>

<span data-ttu-id="9fd8f-179">Konfigurera appinställningar för:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-179">Configure app settings for:</span></span>

* <span data-ttu-id="9fd8f-180">Information om databas</span><span class="sxs-lookup"><span data-stu-id="9fd8f-180">Database information</span></span>
* <span data-ttu-id="9fd8f-181">Aktivera/inaktivera WordPress-loggning</span><span class="sxs-lookup"><span data-stu-id="9fd8f-181">Turning on/off WordPress logging</span></span>
* <span data-ttu-id="9fd8f-182">Säkerhetsinställningar för WordPress</span><span class="sxs-lookup"><span data-stu-id="9fd8f-182">WordPress security settings</span></span>

![App-inställningar för Wordpress-webbapp](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

<span data-ttu-id="9fd8f-184">Kontrollera att du lägger till följande appinställningar för din web app och fas produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-184">Make sure that you add the following app settings for your production web app and stage slot.</span></span> <span data-ttu-id="9fd8f-185">Observera att produktion webbapp och fristående webbprogrammet använda olika databaser.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-185">Note that the production web app and staging web app use different databases.</span></span>

1. <span data-ttu-id="9fd8f-186">Avmarkera den **fack inställningen** kryssrutan för alla inställningar parametrar utom WP_ENV.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-186">Clear the **Slot Setting** checkbox for all the settings parameters except WP_ENV.</span></span> <span data-ttu-id="9fd8f-187">Den här processen kommer växla konfigurationen för din webbapp, innehåll och databasen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-187">This process will swap the configuration for your web app, file content, and database.</span></span> <span data-ttu-id="9fd8f-188">Om **fack inställningen** är markerat, måste webbappen app-inställningar och anslutningssträng konfiguration *inte* flytta mellan miljöer när du gör en **växla** igen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-188">If **Slot Setting** is checked, the web app’s app settings and connection string configuration will *not* move across environments when doing a **Swap** operation.</span></span> <span data-ttu-id="9fd8f-189">Alla ändringar i databasen som finns bryter inte ditt webbprogram för produktion.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-189">Any database changes that are present will not break your production web app.</span></span>

2. <span data-ttu-id="9fd8f-190">Distribuera webbprogram för lokal utveckling miljö till steget webbapp och databasen med hjälp av WebMatrix eller -verktyg, till exempel FTP, Git eller PhpMyAdmin.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-190">Deploy the local development environment web app to the stage web app and database by using WebMatrix or tools of your choice, such as FTP, Git, or PhpMyAdmin.</span></span>

    ![Web Matrix publicera dialogrutan för WordPress-webbapp](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

3. <span data-ttu-id="9fd8f-192">Bläddra och testa din fristående webbapp.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-192">Browse and test your staging web app.</span></span> <span data-ttu-id="9fd8f-193">Med tanke på ett scenario där tema för webbappen är uppdateras är det här fristående webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-193">Considering a scenario where the theme of the web app is to be updated, here is the staging web app.</span></span>

    ![Bläddra mellanlagring webbprogrammet innan du byter fack](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)

4. <span data-ttu-id="9fd8f-195">Om alla ser bra ut klickar du på den **växla** knappen på din fristående webbprogram för att flytta innehållet till produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-195">If all looks good, click the **Swap** button on your staging web app to move your content to the production environment.</span></span> <span data-ttu-id="9fd8f-196">I så fall måste du växla webbappen och databasen i miljöer under varje **växlingen** igen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-196">In this case, you swap the web app and the database across environments during every **Swap** operation.</span></span>

    ![Växla förhandsgranskningen ändras för WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

    > [!NOTE]
    > <span data-ttu-id="9fd8f-198">Om ditt scenario behöver endast push-filer (ingen databasuppdateringar), kontrollera **fack inställningen** för alla de Databasrelaterade *appinställningar* och *anslutning strängar inställningar* i den **Webbprograminställningarna** bladet i Azure-portalen innan du gör den **växla**.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-198">If your scenario needs to only push files (no database updates), then check **Slot Setting** for all the database-related *app settings* and *connection strings settings* in the **Web App Settings** blade within the Azure portal before doing the **Swap**.</span></span> <span data-ttu-id="9fd8f-199">I det här fallet %{db_name/, DB_HOST, DB_PASSWORD, DB_USER och inställningar för standardanslutning sträng ska inte visas i förhandsgranskningen ändras när du gör en **växla**.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-199">In this case, DB_NAME, DB_HOST, DB_PASSWORD, DB_USER, and default connection string settings should not show up in preview changes when you do a **Swap**.</span></span> <span data-ttu-id="9fd8f-200">Just nu, när du har slutfört den **växla** åtgärd WordPress-webbapp har endast uppdateringar filer.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-200">At this time, when you complete the **Swap** operation, the WordPress web app will have the updates files only.</span></span>
    >
    >

    <span data-ttu-id="9fd8f-201">Innan du gör en **växla**, här är produktion WordPress-webbapp.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-201">Before doing a **Swap**, here is the production WordPress web app.</span></span>
    <span data-ttu-id="9fd8f-202">![Produktion webbprogrammet innan du byter fack](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span><span class="sxs-lookup"><span data-stu-id="9fd8f-202">![Production web app before swapping slots](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)</span></span>

    <span data-ttu-id="9fd8f-203">Efter den **växla** åtgärden temat har uppdaterats på ditt webbprogram för produktion.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-203">After the **Swap** operation, the theme has been updated on your production web app.</span></span>

    ![Webbprogram i produktion efter växling fack](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

5. <span data-ttu-id="9fd8f-205">När du behöver återställa går du till produktion web **Appinställningar**, och klicka på den **växlingen** knappen för att växla webbappen och databasen från produktionen till mellanlagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-205">When you need to roll back, you can go to the production web **App Settings**, and click the **Swap** button to swap the web app and database from production to staging slot.</span></span> <span data-ttu-id="9fd8f-206">Tänk på att om ändringar i databasen ingår i en **växla** igen nästa gång du distribuerar till ditt fristående webbprogram som du behöver distribuera databasändringar på den aktuella databasen för din fristående webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-206">Remember that if database changes are included with a **Swap** operation, then the next time you deploy to your staging web app, you need to deploy the database changes to the current database for your staging web app.</span></span> <span data-ttu-id="9fd8f-207">Den aktuella databasen kanske tidigare produktionsdatabasen eller fas-databasen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-207">The current database might be the previous production database or the stage database.</span></span>

#### <a name="summary"></a><span data-ttu-id="9fd8f-208">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9fd8f-208">Summary</span></span>
<span data-ttu-id="9fd8f-209">Följande är en generaliserad process för alla program som har en databas:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-209">Following is a generalized process for any application that has a database:</span></span>

1. <span data-ttu-id="9fd8f-210">Installera programmet på din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-210">Install the application on your local environment.</span></span>
2. <span data-ttu-id="9fd8f-211">Inkludera miljö-specifika konfigurationer (lokala och Azure Web Apps).</span><span class="sxs-lookup"><span data-stu-id="9fd8f-211">Include environment-specific configurations (local and Azure Web Apps).</span></span>
3. <span data-ttu-id="9fd8f-212">Konfigurera dina mellanlagring och produktion miljöer för Web Apps.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-212">Set up your staging and production environments for Web Apps.</span></span>
4. <span data-ttu-id="9fd8f-213">Om du har ett produktionsprogram som redan körs på Azure, synkronisera produktion innehållet (filer/kod och databasen) till lokala och fristående miljöer.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-213">If you have a production application already running on Azure, sync your production content (files/code and database) to local and staging environments.</span></span>
5. <span data-ttu-id="9fd8f-214">Utveckla ditt program på din lokala miljö.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-214">Develop your application on your local environment.</span></span>
6. <span data-ttu-id="9fd8f-215">Placera ditt webbprogram för produktion under underhåll eller låst läge och synkronisera databasen innehåll från produktionen för mellanlagrings- och dev miljöer.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-215">Place your production web app under maintenance or locked mode, and sync database content from production to staging and dev environments.</span></span>
7. <span data-ttu-id="9fd8f-216">Distribuera till mellanlagringsmiljön och testning.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-216">Deploy to the staging environment and test.</span></span>
8. <span data-ttu-id="9fd8f-217">Distribuera till produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-217">Deploy to production environment.</span></span>
9. <span data-ttu-id="9fd8f-218">Upprepa steg 4 till 6.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-218">Repeat steps 4 through 6.</span></span>

### <a name="umbraco"></a><span data-ttu-id="9fd8f-219">Umbraco</span><span class="sxs-lookup"><span data-stu-id="9fd8f-219">Umbraco</span></span>
<span data-ttu-id="9fd8f-220">I det här avsnittet får du lära dig hur Umbraco CMS använder en anpassad modul för att distribuera i miljöer med flera DevOps.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-220">In this section, you will learn how the Umbraco CMS uses a custom module to deploy across multiple DevOps environments.</span></span> <span data-ttu-id="9fd8f-221">Det här exemplet innehåller en annan metod för att hantera flera utvecklingsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-221">This example provides a different approach to managing multiple development environments.</span></span>

<span data-ttu-id="9fd8f-222">[Umbraco CMS](http://umbraco.com/) är en populär .NET CMS-lösning som används av många utvecklare.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-222">[Umbraco CMS](http://umbraco.com/) is a popular .NET CMS solution that's used by many developers.</span></span> <span data-ttu-id="9fd8f-223">Det ger den [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modul för distribution från utvecklingsmiljö till Förproduktion till produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-223">It provides the [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module to deploy from development to staging to production environments.</span></span> <span data-ttu-id="9fd8f-224">Du kan enkelt skapa en lokal utvecklingsmiljö för en Umbraco CMS-webbapp med hjälp av Visual Studio eller WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-224">You can easily create a local development environment for an Umbraco CMS web app by using Visual Studio or WebMatrix.</span></span>

- [<span data-ttu-id="9fd8f-225">Skapa en Umbraco webbprogram med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9fd8f-225">Create an Umbraco web app with Visual Studio</span></span>](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget)
- [<span data-ttu-id="9fd8f-226">Skapa en Umbraco webbapp med WebMatrix</span><span class="sxs-lookup"><span data-stu-id="9fd8f-226">Create an Umbraco web app with WebMatrix</span></span>](http://umbraco.tv/videos/umbraco-v7/implementor/fundamentals/installation/creating-umbraco-site-from-webmatrix-web-gallery/)

<span data-ttu-id="9fd8f-227">Komma ihåg att ta bort den `install` mapp under ditt program och aldrig ladda upp det steg eller produktion webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-227">Always remember to remove the `install` folder under your application, and never upload it to stage or production web apps.</span></span> <span data-ttu-id="9fd8f-228">Den här kursen använder WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-228">This tutorial uses WebMatrix.</span></span>

#### <a name="set-up-a-staging-environment"></a><span data-ttu-id="9fd8f-229">Konfigurera en mellanlagringsmiljön</span><span class="sxs-lookup"><span data-stu-id="9fd8f-229">Set up a staging environment</span></span>
1. <span data-ttu-id="9fd8f-230">Skapa en distributionsplats som tidigare nämnts för webbprogrammet Umbraco CMS, förutsatt att du redan har en Umbraco CMS-webbapp och körs.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-230">Create a deployment slot as mentioned previously for the Umbraco CMS web app, assuming you already have an Umbraco CMS web app up and running.</span></span> <span data-ttu-id="9fd8f-231">Om du inte vill skapa du en från Marketplace.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-231">If you do not, you can create one from the Marketplace.</span></span>
2. <span data-ttu-id="9fd8f-232">Uppdatera anslutningssträngen för din steget distributionsplatsen så att den pekar till den nya **umbraco-steg-db** databas.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-232">Update the connection string for your stage deployment slot to point to the new **umbraco-stage-db** database.</span></span> <span data-ttu-id="9fd8f-233">Ditt webbprogram för produktion (umbraositecms-1) och fristående webbapp (umbracositecms-1-delen) *måste* pekar på olika databaser.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-233">Your production web app (umbraositecms-1) and staging web app (umbracositecms-1-stage) *must* point to different databases.</span></span>

    ![Uppdatera anslutningssträngen för mellanlagring webbprogram med nya tillfälliga databasen](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

3. <span data-ttu-id="9fd8f-235">Klicka på **hämta Publiceringsinställningar** för distributionsplatsen **steget**.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-235">Click **Get Publish settings** for the deployment slot **stage**.</span></span> <span data-ttu-id="9fd8f-236">Den här processen hämtar ett publicera-inställningsfilen som lagrar all information som Visual Studio eller WebMatrix kräver för att publicera programmet från lokal utveckling webbapp till Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-236">This process will download a publish settings file that stores all the information that Visual Studio or WebMatrix requires to publish your application from the local development web app to the Azure web app.</span></span>

    ![Hämta publicera för mellanlagring webbprogram](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)
4. <span data-ttu-id="9fd8f-238">Öppna ditt webbprogram för lokal utveckling i WebMatrix eller Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-238">Open your local development web app in WebMatrix or Visual Studio.</span></span> <span data-ttu-id="9fd8f-239">Den här kursen använder WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-239">This tutorial uses WebMatrix.</span></span> <span data-ttu-id="9fd8f-240">Först måste du importera filen publicera för fristående webbappen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-240">First, you need to import the publish settings file for your staging web app.</span></span>

    ![Importera publiceringsinställningarna för Umbraco med hjälp av Web Matrix](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

5. <span data-ttu-id="9fd8f-242">Granska ändringar i dialogrutan och distribuera webbappen lokalt till din Azure webbapp *umbracositecms 1 steg*.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-242">Review changes in the dialog box, and deploy your local web app to your Azure web app, *umbracositecms-1-stage*.</span></span> <span data-ttu-id="9fd8f-243">När du distribuerar filer direkt till webbappen fristående du utelämnar filer i den `~/app_data/TEMP/` mappen eftersom filerna återskapas när steget webbprogrammet startade.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-243">When you deploy files directly to your staging web app, you will omit files in the `~/app_data/TEMP/` folder because these files will be regenerated when the stage web app is first started.</span></span> <span data-ttu-id="9fd8f-244">Du bör också hoppa över den `~/app_data/umbraco.config` -fil, som också kommer att genereras om.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-244">You should also omit the `~/app_data/umbraco.config` file, which will also be regenerated.</span></span>

    ![Granska publicera ändringar i web matris](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

6. <span data-ttu-id="9fd8f-246">När du har publicerat Umbraco lokala webbappen till mellanlagring webbappen Bläddra till ditt fristående webbprogram och köra några tester för att utesluta eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-246">After you successfully publish the Umbraco local web app to the staging web app, browse to your staging web app, and run a few tests to rule out any issues.</span></span>

#### <a name="set-up-the-courier2-deployment-module"></a><span data-ttu-id="9fd8f-247">Ställa in modulen Courier2 distribution</span><span class="sxs-lookup"><span data-stu-id="9fd8f-247">Set up the Courier2 deployment module</span></span>
<span data-ttu-id="9fd8f-248">Med den [Courier2](http://umbraco.com/products/more-add-ons/courier-2) modulen, högerklickar du bara på att push-innehåll, formatmallar och utveckling moduler från en fristående webbapp till en webbapp i produktion.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-248">With the [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module, you can simply right-click to push content, style sheets, and development modules from a staging web app to a production web app.</span></span> <span data-ttu-id="9fd8f-249">Den här processen minskar risken för att dela ditt webbprogram för produktion när du distribuerar en uppdatering.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-249">This process reduces the risk of breaking your production web app when you deploy an update.</span></span>
<span data-ttu-id="9fd8f-250">Köp en licens för Courier2 för den `*.azurewebsites.net` domän och den anpassade domänen (Säg http://abc.com).</span><span class="sxs-lookup"><span data-stu-id="9fd8f-250">Purchase a license for Courier2 for the `*.azurewebsites.net` domain and your custom domain (say http://abc.com).</span></span> <span data-ttu-id="9fd8f-251">När du har köpt licensen placera hämtade licensen (. LIC-fil) i den `bin` mapp.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-251">After you purchase the license, place the downloaded license (.LIC file) in the `bin` folder.</span></span>

![Släpp licensfil under bin-mappen](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

1. <span data-ttu-id="9fd8f-253">[Hämta Courier2](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span><span class="sxs-lookup"><span data-stu-id="9fd8f-253">[Download the Courier2 package](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/).</span></span> <span data-ttu-id="9fd8f-254">Logga in på ditt steget webbprogram Säg http://umbracocms-site-stage.azurewebsites.net/umbraco klickar du på den **Developer** -menyn och klicka sedan på **paket** > **installationspaket för lokala**.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-254">Sign in to your stage web app, say http://umbracocms-site-stage.azurewebsites.net/umbraco, click the **Developer** menu, and then click **Packages** > **Install local package**.</span></span>

    ![Umbraco installationspaketet](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

2. <span data-ttu-id="9fd8f-256">Överför Courier2 paketet med hjälp av installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-256">Upload the Courier2 package by using the installer.</span></span>

    ![Överföra courier modul-paket](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

3. <span data-ttu-id="9fd8f-258">Om du vill konfigurera paketet, måste du uppdatera filen courier.config under den **Config** för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-258">To configure the package, you need to update the courier.config file under the **Config** folder of your web app.</span></span>

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

4. <span data-ttu-id="9fd8f-259">Under `<repositories>`, ange den produktion plats URL och användare.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-259">Under `<repositories>`, enter the production site URL and user information.</span></span>
    <span data-ttu-id="9fd8f-260">Om du använder Umbraco standardprovider för medlemskap, Lägg sedan till ID: T för Administration av användare i den &lt;användaren&gt; avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-260">If you are using the default Umbraco membership provider, then add the ID for the Administration user in the &lt;user&gt; section.</span></span>
    <span data-ttu-id="9fd8f-261">Om du använder en anpassad Umbraco medlemskapsprovider använda `<login>`,`<password>` i modulen Courier2 att ansluta till produktionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-261">If you are using a custom Umbraco membership provider, use `<login>`,`<password>` in the Courier2 module to connect to the production site.</span></span>
    <span data-ttu-id="9fd8f-262">Mer information [Läs dokumentationen om modulen Courier2](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span><span class="sxs-lookup"><span data-stu-id="9fd8f-262">For more details, [review the documentation for the Courier2 module](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation).</span></span>

5. <span data-ttu-id="9fd8f-263">På motsvarande sätt installera modulen Courier2 på webbplatsen produktion och konfigurera den att peka till webbappen steg i dess respektive courier.config-fil som visas här.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-263">Similarly, install the Courier2 module on your production site, and configure it to point to the stage web app in its respective courier.config file as shown here.</span></span>

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

6. <span data-ttu-id="9fd8f-264">Klicka på den **Courier2** i infopanelen Umbraco CMS web app och klicka sedan på **platser**.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-264">Click the **Courier2** tab in the Umbraco CMS web app dashboard, and then click **Locations**.</span></span> <span data-ttu-id="9fd8f-265">Du bör se databasnamnet som anges i `courier.config`.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-265">You should see the repository name as mentioned in `courier.config`.</span></span> <span data-ttu-id="9fd8f-266">Gör den här processen på din produktion och ett fristående webbprogram.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-266">Do this process on both your production and staging web apps.</span></span>

    ![Visa måldatabasen web app](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

7. <span data-ttu-id="9fd8f-268">För att distribuera innehåll från webbplatsen mellanlagringsplatsen till produktionsplatsen, gå till **innehåll**, och välj en befintlig sida eller skapa en ny sida.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-268">To deploy content from the staging site to the production site, go to **Content**, and select an existing page or create a new page.</span></span> <span data-ttu-id="9fd8f-269">Att välja en befintlig sida från webbappen där titeln på sidan är **komma igång – nya**, och klicka sedan på **spara och publicera**.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-269">I will select an existing page from my web app where the title of the page is **Getting Started – new**, and then click **Save and Publish**.</span></span>

    ![Ändra rubriken på sidan och publicera](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

8. <span data-ttu-id="9fd8f-271">Högerklicka på den aktuella sidan om du vill visa alla alternativ.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-271">Right-click the modified page to view all the options.</span></span> <span data-ttu-id="9fd8f-272">Klicka på **Courier** att öppna den **distribution** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-272">Click **Courier** to open the **Deployment** dialog box.</span></span> <span data-ttu-id="9fd8f-273">Klicka på **distribuera** att initiera distributionen.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-273">Click **Deploy** to initiate deployment.</span></span>

    ![Courier modulen distribution dialogrutan](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

9. <span data-ttu-id="9fd8f-275">Granska ändringar och klicka sedan på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-275">Review the changes, and then click **Continue**.</span></span>

    ![Courier modulen distribution dialogrutan Granska ändringar](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

    <span data-ttu-id="9fd8f-277">Distributionsloggen visar om distributionen lyckades.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-277">The deployment log shows if the deployment was successful.</span></span>

     ![Visa distributionsloggar Courier-modul](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

10. <span data-ttu-id="9fd8f-279">Bläddra i ditt webbprogram för produktion för att se om ändringarna visas.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-279">Browse your production web app to see if the changes are reflected.</span></span>

     ![Bläddra webbprogrammet för produktion](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

<span data-ttu-id="9fd8f-281">Läs dokumentationen för mer information om hur du använder Courier.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-281">To learn more about how to use Courier, review the documentation.</span></span>

#### <a name="how-to-upgrade-the-umbraco-cms-version"></a><span data-ttu-id="9fd8f-282">Så här uppgraderar du Umbraco CMS-version</span><span class="sxs-lookup"><span data-stu-id="9fd8f-282">How to upgrade the Umbraco CMS version</span></span>
<span data-ttu-id="9fd8f-283">Courier kommer inte hjälper dig att uppgradera från en version av Umbraco CMS till en annan.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-283">Courier will not help you upgrade from one version of Umbraco CMS to another.</span></span> <span data-ttu-id="9fd8f-284">När du uppgraderar en Umbraco CMS-version, måste du kontrollera med din anpassade moduler eller moduler från partners och Umbraco Core-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-284">When you upgrade an Umbraco CMS version, you must check for incompatibilities with your custom modules or modules from partners and the Umbraco Core libraries.</span></span> <span data-ttu-id="9fd8f-285">Här är bästa praxis:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-285">Here are best practices:</span></span>

* <span data-ttu-id="9fd8f-286">Säkerhetskopiera alltid din webbappen och databasen innan du uppgraderar.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-286">Always back up your web app and database before you upgrade.</span></span> <span data-ttu-id="9fd8f-287">På web apps i Azure kan du ställa in automatiska säkerhetskopieringar för dina webbplatser med hjälp av funktionen för säkerhetskopiering och återställa platsen om det behövs med hjälp av funktionen för återställning.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-287">On web apps in Azure, you can set up automatic backups for your websites by using the backup feature and restore your site if needed by using the restore feature.</span></span> <span data-ttu-id="9fd8f-288">Mer information finns i [säkerhetskopiering av ditt webbprogram](web-sites-backup.md) och [återställa ditt webbprogram](web-sites-restore.md).</span><span class="sxs-lookup"><span data-stu-id="9fd8f-288">For more details, see [How to back up your web app](web-sites-backup.md) and [How to restore your web app](web-sites-restore.md).</span></span>
* <span data-ttu-id="9fd8f-289">Kontrollera om paket från partner är kompatibel med den version som du uppgraderar till.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-289">Check if packages from partners are compatible with the version you're upgrading to.</span></span> <span data-ttu-id="9fd8f-290">På paketet hämtningssidan, granska projektet kompatibilitet med Umbraco CMS-version.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-290">On the package's download page, review the project compatibility with Umbraco CMS version.</span></span>

<span data-ttu-id="9fd8f-291">Mer information om hur du uppgraderar ditt webbprogram lokalt, [finns allmänna uppgradera](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span><span class="sxs-lookup"><span data-stu-id="9fd8f-291">For more details about how to upgrade your web app locally, [see the general upgrade guidance](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general).</span></span>

<span data-ttu-id="9fd8f-292">Publicera ändringarna till den tillfälliga webbappen när lokal utvecklingsplatsen har uppgraderats.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-292">After your local development site is upgraded, publish the changes to the staging web app.</span></span> <span data-ttu-id="9fd8f-293">Testa ditt program.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-293">Test your application.</span></span> <span data-ttu-id="9fd8f-294">Om alla ser bra ut använder den **växlingen** för att byta ut din fristående plats till produktion webbapp.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-294">If all looks good, use the **Swap** button to swap your staging site to the production web app.</span></span> <span data-ttu-id="9fd8f-295">När du använder den **växla** igen, du kan visa ändringarna som kommer att påverkas i ditt webbprogram konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-295">When you use the **Swap** operation, you can view the changes that will be affected in your web app's configuration.</span></span> <span data-ttu-id="9fd8f-296">Detta **växla** åtgärden växlingar webbappar och databaser.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-296">This **Swap** operation swaps the web apps and databases.</span></span> <span data-ttu-id="9fd8f-297">Efter den **växla**web produktionsprogrammet peka mot umbraco-steg-db-databasen och fristående webbappen att peka umbraco-produktprenumeration-db-databas.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-297">After the **Swap**, the production web app will point to the umbraco-stage-db database, and the staging web app will point to umbraco-prod-db database.</span></span>

![Växla förhandsgranskning för att distribuera Umbraco CMS](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

<span data-ttu-id="9fd8f-299">Här är fördelarna med att byta både webbappen och databasen:</span><span class="sxs-lookup"><span data-stu-id="9fd8f-299">Here are advantages of swapping both the web app and the database:</span></span>

* <span data-ttu-id="9fd8f-300">Du kan återställa till den tidigare versionen av ditt webbprogram med en annan **växla** om det uppstår några problem med programmet.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-300">You can roll back to the previous version of your web app with another **Swap** if there are any application issues.</span></span>
* <span data-ttu-id="9fd8f-301">Du måste distribuera filer och databaser från fristående webbappen till produktion webbappen och databasen för en uppgradering.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-301">For an upgrade, you need to deploy files and databases from the staging web app to the production web app and database.</span></span> <span data-ttu-id="9fd8f-302">Många saker går fel när du distribuerar filer och databaser.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-302">Many things can go wrong when you deploy files and databases.</span></span> <span data-ttu-id="9fd8f-303">Med hjälp av den **växla** funktionen fack, kan vi minska avbrottstiden vid en uppgradering och minska risken för fel som kan uppstå när du distribuerar ändringar.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-303">By using the **Swap** feature of slots, we can reduce downtime during an upgrade and reduce the risk of failures that can occur when you deploy changes.</span></span>
* <span data-ttu-id="9fd8f-304">Du kan göra **A / B-testning** med hjälp av den [test i produktion](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) funktion.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-304">You can do **A/B testing** by using the [Testing in production](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) feature.</span></span>

<span data-ttu-id="9fd8f-305">Det här exemplet visar flexibilitet i plattformen där du kan skapa anpassade moduler som liknar Umbraco Courier modul som ska hantera distribution mellan miljöer.</span><span class="sxs-lookup"><span data-stu-id="9fd8f-305">This example shows you the flexibility of the platform where you can build custom modules similar to Umbraco Courier module to manage deployment across environments.</span></span>

## <a name="references"></a><span data-ttu-id="9fd8f-306">Referenser</span><span class="sxs-lookup"><span data-stu-id="9fd8f-306">References</span></span>
[<span data-ttu-id="9fd8f-307">Flexibel programvaruutveckling med Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9fd8f-307">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)

[<span data-ttu-id="9fd8f-308">Skapa mellanlagringsmiljöer för web apps i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9fd8f-308">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)

[<span data-ttu-id="9fd8f-309">Blockera Webbåtkomst till icke-produktion distributionsplatser</span><span class="sxs-lookup"><span data-stu-id="9fd8f-309">How to block web access to non-production deployment slots</span></span>](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
