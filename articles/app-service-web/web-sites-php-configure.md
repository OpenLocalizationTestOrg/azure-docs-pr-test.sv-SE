---
title: "Så här konfigurerar du PHP i Azure App Service Web Apps | Microsoft Docs"
description: "Lär dig hur du konfigurerar PHP-standardinstallation eller lägga till en anpassad PHP-installation för Web Apps i Azure App Service."
services: app-service
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 624dd416f37aacdb3d2f6e59afdc2efe646e610b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-php-in-azure-app-service-web-apps"></a><span data-ttu-id="1466d-103">Så här konfigurerar du PHP i Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="1466d-103">Configure PHP in Azure App Service Web Apps</span></span>
## <a name="introduction"></a><span data-ttu-id="1466d-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="1466d-104">Introduction</span></span>
<span data-ttu-id="1466d-105">Den här guiden visar hur du konfigurerar för inbyggda PHP-körning för Web Apps i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)anger en anpassad PHP-körning och aktivera tillägg.</span><span class="sxs-lookup"><span data-stu-id="1466d-105">This guide will show you how to configure the built-in PHP runtime for Web Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), provide a custom PHP runtime, and enable extensions.</span></span> <span data-ttu-id="1466d-106">Om du vill använda Apptjänst, registrera dig för den [kostnadsfri utvärderingsversion].</span><span class="sxs-lookup"><span data-stu-id="1466d-106">To use App Service, sign up for the [free trial].</span></span> <span data-ttu-id="1466d-107">För att få ut mesta möjliga av den här guiden bör du först skapa en PHP-webbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="1466d-107">To get the most from this guide, you should first create a PHP web app in App Service.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-the-built-in-php-version"></a><span data-ttu-id="1466d-108">Så här: ändra inbyggda PHP-version</span><span class="sxs-lookup"><span data-stu-id="1466d-108">How to: Change the built-in PHP version</span></span>
<span data-ttu-id="1466d-109">Som standard är PHP 5.5 installerat och omedelbart tillgängliga för användning när du skapar en webbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="1466d-109">By default, PHP 5.5 is installed and immediately available for use when you create an App Service web app.</span></span> <span data-ttu-id="1466d-110">Det bästa sättet att se tillgängliga versionen revision, standardkonfigurationen och aktiverade tillägg är att distribuera ett skript som anropar den [phpinfo()] funktion.</span><span class="sxs-lookup"><span data-stu-id="1466d-110">The best way to see the available release revision, its default configuration, and the enabled extensions is to deploy a script that calls the [phpinfo()] function.</span></span>

<span data-ttu-id="1466d-111">PHP 5.6 och PHP 7.0 versioner är också tillgängliga, men inte aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="1466d-111">PHP 5.6 and PHP 7.0 versions are also available, but not enabled by default.</span></span> <span data-ttu-id="1466d-112">Uppdatera PHP-version, så en av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="1466d-112">To update the PHP version, follow one of these methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="1466d-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="1466d-113">Azure Portal</span></span>
1. <span data-ttu-id="1466d-114">Bläddra till ditt webbprogram i den [Azure Portal](https://portal.azure.com) och klicka på den **inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="1466d-114">Browse to your web app in the [Azure Portal](https://portal.azure.com) and click on the **Settings** button.</span></span>
   
    ![Webbprograminställningarna][settings-button]
2. <span data-ttu-id="1466d-116">Från den **inställningar** bladet välj **programinställningar** och välj den nya versionen av PHP.</span><span class="sxs-lookup"><span data-stu-id="1466d-116">From the **Settings** blade select **Application Settings** and choose the new PHP version.</span></span>
   
    ![Programinställningar][application-settings]
3. <span data-ttu-id="1466d-118">Klicka på den **spara** längst upp i den **Web app-inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="1466d-118">Click the **Save** button at the top of the **Web app settings** blade.</span></span>
   
    ![Spara inställningar][save-button]

### <a name="azure-powershell-windows"></a><span data-ttu-id="1466d-120">Azure PowerShell (Windows)</span><span class="sxs-lookup"><span data-stu-id="1466d-120">Azure PowerShell (Windows)</span></span>
1. <span data-ttu-id="1466d-121">Öppna Azure PowerShell, och logga in på ditt konto:</span><span class="sxs-lookup"><span data-stu-id="1466d-121">Open Azure PowerShell, and login to your account:</span></span>
   
        PS C:\> Login-AzureRmAccount
2. <span data-ttu-id="1466d-122">Ange PHP-version för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1466d-122">Set the PHP version for the web app.</span></span>
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. <span data-ttu-id="1466d-123">PHP-version anges nu.</span><span class="sxs-lookup"><span data-stu-id="1466d-123">The PHP version is now set.</span></span> <span data-ttu-id="1466d-124">Du kan bekräfta inställningarna:</span><span class="sxs-lookup"><span data-stu-id="1466d-124">You can confirm these settings:</span></span>
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a><span data-ttu-id="1466d-125">Azure-kommandoradsgränssnittet (Linux, Mac, Windows)</span><span class="sxs-lookup"><span data-stu-id="1466d-125">Azure Command-Line Interface (Linux, Mac, Windows)</span></span>
<span data-ttu-id="1466d-126">Du måste ha för att använda Azure-kommandoradsgränssnittet **Node.js** installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="1466d-126">To use the Azure Command-Line Interface, you must have **Node.js** installed on your computer.</span></span>

1. <span data-ttu-id="1466d-127">Öppna Terminal och logga in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="1466d-127">Open Terminal, and login to your account.</span></span>
   
        azure login
2. <span data-ttu-id="1466d-128">Ange PHP-version för webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1466d-128">Set the PHP version for the web app.</span></span>
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. <span data-ttu-id="1466d-129">PHP-version anges nu.</span><span class="sxs-lookup"><span data-stu-id="1466d-129">The PHP version is now set.</span></span> <span data-ttu-id="1466d-130">Du kan bekräfta inställningarna:</span><span class="sxs-lookup"><span data-stu-id="1466d-130">You can confirm these settings:</span></span>
   
        azure site show {app-name}

> [!NOTE] 
> <span data-ttu-id="1466d-131">Den [Azure CLI 2.0](https://github.com/Azure/azure-cli) kommandon som motsvarar det som nämns ovan:</span><span class="sxs-lookup"><span data-stu-id="1466d-131">The [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands that are equivalent to the above are:</span></span>
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-the-built-in-php-configurations"></a><span data-ttu-id="1466d-132">Så här: ändra inbyggda PHP-konfigurationer</span><span class="sxs-lookup"><span data-stu-id="1466d-132">How to: Change the built-in PHP configurations</span></span>
<span data-ttu-id="1466d-133">Du kan ändra någon av konfigurationsalternativ genom att följa stegen nedan för alla inbyggda PHP-körning.</span><span class="sxs-lookup"><span data-stu-id="1466d-133">For any built-in PHP runtime, you can change any of the configuration options by following the steps below.</span></span> <span data-ttu-id="1466d-134">(Mer information om php.ini-direktiven finns [förteckning över php.ini-direktiv].)</span><span class="sxs-lookup"><span data-stu-id="1466d-134">(For information about php.ini directives, see [List of php.ini directives].)</span></span>

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a><span data-ttu-id="1466d-135">Ändra PHP\_INI\_användare, PHP\_INI\_PERDIR, PHP\_INI\_alla konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="1466d-135">Changing PHP\_INI\_USER, PHP\_INI\_PERDIR, PHP\_INI\_ALL configuration settings</span></span>
1. <span data-ttu-id="1466d-136">Lägg till en [. user.ini] filen till din rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="1466d-136">Add a [.user.ini] file to your root directory.</span></span>
2. <span data-ttu-id="1466d-137">Lägg till konfigurationsinställningar till de `.user.ini` fil med samma syntax som du vill använda i en `php.ini` fil.</span><span class="sxs-lookup"><span data-stu-id="1466d-137">Add configuration settings to the `.user.ini` file using the same syntax you would use in a `php.ini` file.</span></span> <span data-ttu-id="1466d-138">Till exempel om du vill aktivera den `display_errors` inställningen vidare samt anger `upload_max_filesize` till 10M din `.user.ini` filen innehåller den här texten:</span><span class="sxs-lookup"><span data-stu-id="1466d-138">For example, if you wanted to turn the `display_errors` setting on and set `upload_max_filesize` setting to 10M, your `.user.ini` file would contain this text:</span></span>
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
3. <span data-ttu-id="1466d-139">Distribuera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1466d-139">Deploy your web app.</span></span>
4. <span data-ttu-id="1466d-140">Starta om webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1466d-140">Restart the web app.</span></span> <span data-ttu-id="1466d-141">(Omstart krävs eftersom frekvensen med vilken PHP läser `.user.ini` filer styrs av den `user_ini.cache_ttl` som är en inställning av system och 300 sekunder (fem minuter) som standard.</span><span class="sxs-lookup"><span data-stu-id="1466d-141">(Restarting is necessary because the frequency with which PHP reads `.user.ini` files is governed by the `user_ini.cache_ttl` setting, which is a system level setting and is 300 seconds (5 minutes) by default.</span></span> <span data-ttu-id="1466d-142">Starta om webbprogrammet tvingar PHP för att läsa de nya inställningarna i den `.user.ini` filen.)</span><span class="sxs-lookup"><span data-stu-id="1466d-142">Restarting the web app forces PHP to read the new settings in the `.user.ini` file.)</span></span>

<span data-ttu-id="1466d-143">Som ett alternativ till att använda en `.user.ini` filen som du kan använda den [ini_set()] funktion i skript för att ange konfigurationsalternativ som inte är på systemnivå direktiven.</span><span class="sxs-lookup"><span data-stu-id="1466d-143">As an alternative to using a `.user.ini` file, you can use the [ini_set()] function in scripts to set configuration options that are not system-level directives.</span></span>

### <a name="changing-phpinisystem-configuration-settings"></a><span data-ttu-id="1466d-144">Ändra PHP\_INI\_SYSTEM konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="1466d-144">Changing PHP\_INI\_SYSTEM configuration settings</span></span>
1. <span data-ttu-id="1466d-145">Lägg till en Appinställning i ditt webbprogram med nyckeln `PHP_INI_SCAN_DIR` och värde`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="1466d-145">Add an App Setting to your Web App with the key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
2. <span data-ttu-id="1466d-146">Skapa en `settings.ini` filen med Kudu-konsolen (http://&lt;site-name&gt;. scm.azurewebsite.net) i den `d:\home\site\ini` directory.</span><span class="sxs-lookup"><span data-stu-id="1466d-146">Create an `settings.ini` file using Kudu Console (http://&lt;site-name&gt;.scm.azurewebsite.net) in the `d:\home\site\ini` directory.</span></span>
3. <span data-ttu-id="1466d-147">Lägg till konfigurationsinställningar till de `settings.ini` fil med samma syntax som du vill använda i en php.ini-fil.</span><span class="sxs-lookup"><span data-stu-id="1466d-147">Add configuration settings to the `settings.ini` file using the same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="1466d-148">Om du vill peka till exempel den `curl.cainfo` till en `*.crt` fil och anger ”wincache.maxfilesize-inställningen till 512 kB din `settings.ini` filen innehåller den här texten:</span><span class="sxs-lookup"><span data-stu-id="1466d-148">For example, if you wanted to point the `curl.cainfo` setting to a `*.crt` file and set 'wincache.maxfilesize' setting to 512K, your `settings.ini` file would contain this text:</span></span>
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. <span data-ttu-id="1466d-149">Starta om ditt webbprogram för att läsa in ändringarna.</span><span class="sxs-lookup"><span data-stu-id="1466d-149">Restart your Web App to load the changes.</span></span>

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a><span data-ttu-id="1466d-150">Så här: aktivera tillägg i standard PHP-körning</span><span class="sxs-lookup"><span data-stu-id="1466d-150">How to: Enable extensions in the default PHP runtime</span></span>
<span data-ttu-id="1466d-151">Enligt beskrivningen i föregående avsnitt, det bästa sättet att se PHP standardversionen, standardkonfigurationen och aktiverade tillägg är att distribuera ett skript som anropar [phpinfo()].</span><span class="sxs-lookup"><span data-stu-id="1466d-151">As noted in the previous section, the best way to see the default PHP version, its default configuration, and the enabled extensions is to deploy a script that calls [phpinfo()].</span></span> <span data-ttu-id="1466d-152">Följ stegen nedan om du vill aktivera ytterligare tillägg:</span><span class="sxs-lookup"><span data-stu-id="1466d-152">To enable additional extensions, follow the steps below:</span></span>

### <a name="configure-via-ini-settings"></a><span data-ttu-id="1466d-153">Konfigurera via ini-inställningar</span><span class="sxs-lookup"><span data-stu-id="1466d-153">Configure via ini settings</span></span>
1. <span data-ttu-id="1466d-154">Lägg till en `ext` katalogen på `d:\home\site` directory.</span><span class="sxs-lookup"><span data-stu-id="1466d-154">Add a `ext` directory to the `d:\home\site` directory.</span></span>
2. <span data-ttu-id="1466d-155">Placera `.dll` tillägget filer i den `ext` katalog (till exempel `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="1466d-155">Put `.dll` extension files in the `ext` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="1466d-156">Se till att tilläggen är kompatibel med standardversionen av PHP och är VC9 och icke-trådsäkra (nts)-kompatibel.</span><span class="sxs-lookup"><span data-stu-id="1466d-156">Make sure that the extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="1466d-157">Lägg till en Appinställning i ditt webbprogram med nyckeln `PHP_INI_SCAN_DIR` och värde`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="1466d-157">Add an App Setting to your Web App with the key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
4. <span data-ttu-id="1466d-158">Skapa en `ini` filen i `d:\home\site\ini` kallas `extensions.ini`.</span><span class="sxs-lookup"><span data-stu-id="1466d-158">Create an `ini` file in `d:\home\site\ini` called `extensions.ini`.</span></span>
5. <span data-ttu-id="1466d-159">Lägg till konfigurationsinställningar till de `extensions.ini` fil med samma syntax som du vill använda i en php.ini-fil.</span><span class="sxs-lookup"><span data-stu-id="1466d-159">Add configuration settings to the `extensions.ini` file using the same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="1466d-160">Om du vill aktivera MongoDB och XDebug tillägg, till exempel din `extensions.ini` filen innehåller den här texten:</span><span class="sxs-lookup"><span data-stu-id="1466d-160">For example, if you wanted to enable the MongoDB and XDebug extensions, your `extensions.ini` file would contain this text:</span></span>
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. <span data-ttu-id="1466d-161">Starta om ditt webbprogram för att läsa in ändringarna.</span><span class="sxs-lookup"><span data-stu-id="1466d-161">Restart your Web App to load the changes.</span></span>

### <a name="configure-via-app-setting"></a><span data-ttu-id="1466d-162">Konfigurera via Appinställningen</span><span class="sxs-lookup"><span data-stu-id="1466d-162">Configure via App Setting</span></span>
1. <span data-ttu-id="1466d-163">Lägg till en `bin` katalogen till rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="1466d-163">Add a `bin` directory to the root directory.</span></span>
2. <span data-ttu-id="1466d-164">Placera `.dll` tillägget filer i den `bin` katalog (till exempel `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="1466d-164">Put `.dll` extension files in the `bin` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="1466d-165">Se till att tilläggen är kompatibel med standardversionen av PHP och är VC9 och icke-trådsäkra (nts)-kompatibel.</span><span class="sxs-lookup"><span data-stu-id="1466d-165">Make sure that the extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="1466d-166">Distribuera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1466d-166">Deploy your web app.</span></span>
4. <span data-ttu-id="1466d-167">Bläddra till webbappen i Azure Portal och klicka på den **inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="1466d-167">Browse to your web app in the Azure Portal and click on the **Settings** button.</span></span>
   
    ![Webbprograminställningarna][settings-button]
5. <span data-ttu-id="1466d-169">Från den **inställningar** bladet välj **programinställningar** och bläddra till den **appinställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1466d-169">From the **Settings** blade select **Application Settings** and scroll to the **App settings** section.</span></span>
6. <span data-ttu-id="1466d-170">I den **appinställningar** avsnittet, skapa en **PHP_EXTENSIONS** nyckel.</span><span class="sxs-lookup"><span data-stu-id="1466d-170">In the **App settings** section, create a **PHP_EXTENSIONS** key.</span></span> <span data-ttu-id="1466d-171">Värdet för den här nyckeln skulle vara en sökväg i förhållande till webbplatsroten: **bin\your ext fil**.</span><span class="sxs-lookup"><span data-stu-id="1466d-171">The value for this key would be a path relative to website root: **bin\your-ext-file**.</span></span>
   
    ![Aktivera tillägget på app-inställningar][php-extensions]
7. <span data-ttu-id="1466d-173">Klicka på den **spara** längst upp i den **Web app-inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="1466d-173">Click the **Save** button at the top of the **Web app settings** blade.</span></span>
   
    ![Spara inställningar][save-button]

<span data-ttu-id="1466d-175">Zend tillägg stöds också med hjälp av en **PHP_ZENDEXTENSIONS** nyckel.</span><span class="sxs-lookup"><span data-stu-id="1466d-175">Zend extensions are also supported by using a **PHP_ZENDEXTENSIONS** key.</span></span> <span data-ttu-id="1466d-176">Om du vill aktivera flera tillägg innehåller en kommaavgränsad lista över `.dll` filer för app inställningens värde.</span><span class="sxs-lookup"><span data-stu-id="1466d-176">To enable multiple extensions, include a comma-separated list of `.dll` files for the app setting value.</span></span>

## <a name="how-to-use-a-custom-php-runtime"></a><span data-ttu-id="1466d-177">Så här: använda en anpassad PHP-körning</span><span class="sxs-lookup"><span data-stu-id="1466d-177">How to: Use a custom PHP runtime</span></span>
<span data-ttu-id="1466d-178">I stället för standard PHP-körning kan App Service Web Apps använda en PHP-körning som du anger för att köra PHP-skript.</span><span class="sxs-lookup"><span data-stu-id="1466d-178">Instead of the default PHP runtime, App Service Web Apps can use a PHP runtime that you provide to execute PHP scripts.</span></span> <span data-ttu-id="1466d-179">Körningsmiljön som du anger kan konfigureras med en `php.ini` -fil som du också ange.</span><span class="sxs-lookup"><span data-stu-id="1466d-179">The runtime that you provide can be configured by a `php.ini` file that you also provide.</span></span> <span data-ttu-id="1466d-180">Följ stegen nedan om du vill använda en anpassad PHP-körning med Web Apps.</span><span class="sxs-lookup"><span data-stu-id="1466d-180">To use a custom PHP runtime with Web Apps, follow the steps below.</span></span>

1. <span data-ttu-id="1466d-181">Hämta en icke-trådsäkra, VC9 eller VC11 kompatibel version av PHP för Windows.</span><span class="sxs-lookup"><span data-stu-id="1466d-181">Obtain a non-thread-safe, VC9 or VC11 compatible version of PHP for Windows.</span></span> <span data-ttu-id="1466d-182">Nya versioner av PHP för Windows finns här: [http://windows.php.net/download/].</span><span class="sxs-lookup"><span data-stu-id="1466d-182">Recent releases of PHP for Windows can be found here: [http://windows.php.net/download/].</span></span> <span data-ttu-id="1466d-183">Äldre versioner finns i det här arkivet: [http://windows.php.net/downloads/releases/archives/].</span><span class="sxs-lookup"><span data-stu-id="1466d-183">Older releases can be found in the archive here: [http://windows.php.net/downloads/releases/archives/].</span></span>
2. <span data-ttu-id="1466d-184">Ändra den `php.ini` filen för din runtime.</span><span class="sxs-lookup"><span data-stu-id="1466d-184">Modify the `php.ini` file for your runtime.</span></span> <span data-ttu-id="1466d-185">Observera att alla inställningar som är endast system-nivå direktiv ignoreras av Web Apps.</span><span class="sxs-lookup"><span data-stu-id="1466d-185">Note that any configuration settings that are system-level-only directives will be ignored by Web Apps.</span></span> <span data-ttu-id="1466d-186">(Mer information om endast system-nivå direktiven finns [förteckning över php.ini-direktiv]).</span><span class="sxs-lookup"><span data-stu-id="1466d-186">(For information about system-level-only directives, see [List of php.ini directives]).</span></span>
3. <span data-ttu-id="1466d-187">Du kan också lägga till tillägg för PHP-körning och aktivera dem i den `php.ini` filen.</span><span class="sxs-lookup"><span data-stu-id="1466d-187">Optionally, add extensions to your PHP runtime and enable them in the `php.ini` file.</span></span>
4. <span data-ttu-id="1466d-188">Lägg till en `bin` katalogen till rotkatalogen och placera den katalog där PHP-körning i den (till exempel `bin\php`).</span><span class="sxs-lookup"><span data-stu-id="1466d-188">Add a `bin` directory to your root directory, and put the directory that contains your PHP runtime in it (for example, `bin\php`).</span></span>
5. <span data-ttu-id="1466d-189">Distribuera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1466d-189">Deploy your web app.</span></span>
6. <span data-ttu-id="1466d-190">Bläddra till webbappen i Azure Portal och klicka på den **inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="1466d-190">Browse to your web app in the Azure Portal and click on the **Settings** button.</span></span>
   
    ![Webbprograminställningarna][settings-button]
7. <span data-ttu-id="1466d-192">Från den **inställningar** bladet välj **programinställningar** och bläddra till den **Hanterarmappningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1466d-192">From the **Settings** blade select **Application Settings** and scroll to the **Handler mappings** section.</span></span> <span data-ttu-id="1466d-193">Lägg till `*.php` till tillägget och Lägg till sökvägen till den `php-cgi.exe` körbara.</span><span class="sxs-lookup"><span data-stu-id="1466d-193">Add `*.php` to the Extension field and add the path to the `php-cgi.exe` executable.</span></span> <span data-ttu-id="1466d-194">Om du placerar PHP-körning i den `bin` katalogen i roten av du programmet sökvägen ska vara `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span><span class="sxs-lookup"><span data-stu-id="1466d-194">If you put your PHP runtime in the `bin` directory in the root of you application, the path will be `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span></span>
   
    ![Ange hanterare i hanterare mappningar][handler-mappings]
8. <span data-ttu-id="1466d-196">Klicka på den **spara** längst upp i den **Web app-inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="1466d-196">Click the **Save** button at the top of the **Web app settings** blade.</span></span>
   
    ![Spara inställningar][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a><span data-ttu-id="1466d-198">Så här: aktivera Composer automatisering i Azure</span><span class="sxs-lookup"><span data-stu-id="1466d-198">How to: Enable Composer automation in Azure</span></span>
<span data-ttu-id="1466d-199">Som standard göra Apptjänst inte något med composer.json, om du har en i PHP-projektet.</span><span class="sxs-lookup"><span data-stu-id="1466d-199">By default, App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="1466d-200">Om du använder [Git-distribution](app-service-deploy-local-git.md), kan du aktivera composer.json bearbetning under `git push` genom att aktivera tillägget Composer.</span><span class="sxs-lookup"><span data-stu-id="1466d-200">If you use [Git deployment](app-service-deploy-local-git.md), you can enable composer.json processing during `git push` by enabling the Composer extension.</span></span>

> [!NOTE]
> <span data-ttu-id="1466d-201">Du kan [rösta på förstklassigt Composer stöd i App Service här](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span><span class="sxs-lookup"><span data-stu-id="1466d-201">You can [vote for first-class Composer support in App Service here](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span></span>
> 
> 

1. <span data-ttu-id="1466d-202">I din PHP web appens blad i den [Azure-portalen](https://portal.azure.com), klickar du på **verktyg** > **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="1466d-202">In your PHP web app's blade in the [Azure portal](https://portal.azure.com), click **Tools** > **Extensions**.</span></span>
   
    ![Azure Portal inställningsbladet att aktivera Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-settings.png)
2. <span data-ttu-id="1466d-204">Klicka på **Lägg till**, klicka på **Composer**.</span><span class="sxs-lookup"><span data-stu-id="1466d-204">Click **Add**, then click **Composer**.</span></span>
   
    ![Lägga till tillägget Composer om du vill aktivera Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-add.png)
3. <span data-ttu-id="1466d-206">Klicka på **OK** att acceptera villkoren.</span><span class="sxs-lookup"><span data-stu-id="1466d-206">Click **OK** to accept legal terms.</span></span> <span data-ttu-id="1466d-207">Klicka på **OK** igen för att lägga till filnamnstillägget.</span><span class="sxs-lookup"><span data-stu-id="1466d-207">Click **OK** again to add the extension.</span></span>
   
    <span data-ttu-id="1466d-208">Den **installerat tillägg** bladet visas nu tillägget Composer.</span><span class="sxs-lookup"><span data-stu-id="1466d-208">The **Installed extensions** blade will now show the Composer extension.</span></span>  
    <span data-ttu-id="1466d-209">![Acceptera villkoren för att aktivera Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-view.png)</span><span class="sxs-lookup"><span data-stu-id="1466d-209">![Accept legal terms to enable Composer automation in Azure](./media/web-sites-php-configure/composer-extension-view.png)</span></span>
4. <span data-ttu-id="1466d-210">Nu kan utföra `git add`, `git commit`, och `git push` precis som i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1466d-210">Now, perform `git add`, `git commit`, and `git push` like in the previous section.</span></span> <span data-ttu-id="1466d-211">Du ser nu att installeras Composer beroenden som definierats i composer.json.</span><span class="sxs-lookup"><span data-stu-id="1466d-211">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Git-distribution med Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a><span data-ttu-id="1466d-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1466d-213">Next steps</span></span>
<span data-ttu-id="1466d-214">Mer information finns i [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="1466d-214">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

> [!NOTE]
> <span data-ttu-id="1466d-215">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="1466d-215">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="1466d-216">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="1466d-216">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="1466d-217">[kostnadsfri utvärderingsversion]: https://www.windowsazure.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="1466d-217">[free trial]: https://www.windowsazure.com/pricing/free-trial/</span></span>
<span data-ttu-id="1466d-218">[phpinfo()]: http://php.net/manual/en/function.phpinfo.php</span><span class="sxs-lookup"><span data-stu-id="1466d-218">[phpinfo()]: http://php.net/manual/en/function.phpinfo.php</span></span>
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
<span data-ttu-id="1466d-219">[förteckning över php.ini-direktiv]: http://www.php.net/manual/en/ini.list.php</span><span class="sxs-lookup"><span data-stu-id="1466d-219">[List of php.ini directives]: http://www.php.net/manual/en/ini.list.php</span></span>
<span data-ttu-id="1466d-220">[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php</span><span class="sxs-lookup"><span data-stu-id="1466d-220">[.user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php</span></span>
<span data-ttu-id="1466d-221">[ini_set()]: http://www.php.net/manual/en/function.ini-set.php</span><span class="sxs-lookup"><span data-stu-id="1466d-221">[ini_set()]: http://www.php.net/manual/en/function.ini-set.php</span></span>
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
<span data-ttu-id="1466d-222">[http://windows.php.net/download/]: http://windows.php.net/download/</span><span class="sxs-lookup"><span data-stu-id="1466d-222">[http://windows.php.net/download/]: http://windows.php.net/download/</span></span>
<span data-ttu-id="1466d-223">[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/</span><span class="sxs-lookup"><span data-stu-id="1466d-223">[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/</span></span>
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png

