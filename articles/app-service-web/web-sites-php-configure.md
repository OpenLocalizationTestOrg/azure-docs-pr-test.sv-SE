---
title: aaaConfigure PHP i Azure App Service Web Apps | Microsoft Docs
description: "Lär dig hur tooconfigure hello standard PHP-installation eller Lägg till en anpassad PHP-installation för Web Apps i Azure App Service."
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
ms.openlocfilehash: 2e461e4a269a4ce5614f5f05560f38bc53066251
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-php-in-azure-app-service-web-apps"></a><span data-ttu-id="d05a0-103">Så här konfigurerar du PHP i Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="d05a0-103">Configure PHP in Azure App Service Web Apps</span></span>
## <a name="introduction"></a><span data-ttu-id="d05a0-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="d05a0-104">Introduction</span></span>
<span data-ttu-id="d05a0-105">Den här guiden visar hur tooconfigure hello inbyggda PHP-körning för Web Apps i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)anger en anpassad PHP-körning och aktivera tillägg.</span><span class="sxs-lookup"><span data-stu-id="d05a0-105">This guide will show you how tooconfigure hello built-in PHP runtime for Web Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), provide a custom PHP runtime, and enable extensions.</span></span> <span data-ttu-id="d05a0-106">toouse Apptjänst, registrera dig för hello [kostnadsfri utvärderingsversion].</span><span class="sxs-lookup"><span data-stu-id="d05a0-106">toouse App Service, sign up for hello [free trial].</span></span> <span data-ttu-id="d05a0-107">tooget hello mest från den här guiden du först skapa en PHP-webbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="d05a0-107">tooget hello most from this guide, you should first create a PHP web app in App Service.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-hello-built-in-php-version"></a><span data-ttu-id="d05a0-108">Så här: ändra hello inbyggda PHP-version</span><span class="sxs-lookup"><span data-stu-id="d05a0-108">How to: Change hello built-in PHP version</span></span>
<span data-ttu-id="d05a0-109">Som standard är PHP 5.5 installerat och omedelbart tillgängliga för användning när du skapar en webbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="d05a0-109">By default, PHP 5.5 is installed and immediately available for use when you create an App Service web app.</span></span> <span data-ttu-id="d05a0-110">Hej bästa sätt toosee hello tillgängliga versionen revision, standardkonfigurationen och hello aktiverade tillägg är toodeploy ett skript som anropar hello [phpinfo()] funktion.</span><span class="sxs-lookup"><span data-stu-id="d05a0-110">hello best way toosee hello available release revision, its default configuration, and hello enabled extensions is toodeploy a script that calls hello [phpinfo()] function.</span></span>

<span data-ttu-id="d05a0-111">PHP 5.6 och PHP 7.0 versioner är också tillgängliga, men inte aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="d05a0-111">PHP 5.6 and PHP 7.0 versions are also available, but not enabled by default.</span></span> <span data-ttu-id="d05a0-112">tooupdate hello PHP-version, Följ någon av följande metoder:</span><span class="sxs-lookup"><span data-stu-id="d05a0-112">tooupdate hello PHP version, follow one of these methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="d05a0-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d05a0-113">Azure Portal</span></span>
1. <span data-ttu-id="d05a0-114">Bläddra tooyour webbprogram i hello [Azure Portal](https://portal.azure.com) och klicka på hello **inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="d05a0-114">Browse tooyour web app in hello [Azure Portal](https://portal.azure.com) and click on hello **Settings** button.</span></span>
   
    ![Webbprograminställningarna][settings-button]
2. <span data-ttu-id="d05a0-116">Från hello **inställningar** bladet välj **programinställningar** och välj hello nya PHP-versionen.</span><span class="sxs-lookup"><span data-stu-id="d05a0-116">From hello **Settings** blade select **Application Settings** and choose hello new PHP version.</span></span>
   
    ![Programinställningar][application-settings]
3. <span data-ttu-id="d05a0-118">Klicka på hello **spara** knappen hello överst i hello **Web app-inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="d05a0-118">Click hello **Save** button at hello top of hello **Web app settings** blade.</span></span>
   
    ![Spara inställningar][save-button]

### <a name="azure-powershell-windows"></a><span data-ttu-id="d05a0-120">Azure PowerShell (Windows)</span><span class="sxs-lookup"><span data-stu-id="d05a0-120">Azure PowerShell (Windows)</span></span>
1. <span data-ttu-id="d05a0-121">Öppna Azure PowerShell och tooyour inloggningskonto:</span><span class="sxs-lookup"><span data-stu-id="d05a0-121">Open Azure PowerShell, and login tooyour account:</span></span>
   
        PS C:\> Login-AzureRmAccount
2. <span data-ttu-id="d05a0-122">Ange hello PHP-version för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d05a0-122">Set hello PHP version for hello web app.</span></span>
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. <span data-ttu-id="d05a0-123">hello PHP-version anges nu.</span><span class="sxs-lookup"><span data-stu-id="d05a0-123">hello PHP version is now set.</span></span> <span data-ttu-id="d05a0-124">Du kan bekräfta inställningarna:</span><span class="sxs-lookup"><span data-stu-id="d05a0-124">You can confirm these settings:</span></span>
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a><span data-ttu-id="d05a0-125">Azure-kommandoradsgränssnittet (Linux, Mac, Windows)</span><span class="sxs-lookup"><span data-stu-id="d05a0-125">Azure Command-Line Interface (Linux, Mac, Windows)</span></span>
<span data-ttu-id="d05a0-126">toouse hello Azure kommandoradsgränssnitt måste du ha **Node.js** installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="d05a0-126">toouse hello Azure Command-Line Interface, you must have **Node.js** installed on your computer.</span></span>

1. <span data-ttu-id="d05a0-127">Öppna Terminal och tooyour inloggningskonto.</span><span class="sxs-lookup"><span data-stu-id="d05a0-127">Open Terminal, and login tooyour account.</span></span>
   
        azure login
2. <span data-ttu-id="d05a0-128">Ange hello PHP-version för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d05a0-128">Set hello PHP version for hello web app.</span></span>
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. <span data-ttu-id="d05a0-129">hello PHP-version anges nu.</span><span class="sxs-lookup"><span data-stu-id="d05a0-129">hello PHP version is now set.</span></span> <span data-ttu-id="d05a0-130">Du kan bekräfta inställningarna:</span><span class="sxs-lookup"><span data-stu-id="d05a0-130">You can confirm these settings:</span></span>
   
        azure site show {app-name}

> [!NOTE] 
> <span data-ttu-id="d05a0-131">Hej [Azure CLI 2.0](https://github.com/Azure/azure-cli) kommandon som är likvärdiga toohello ovan är:</span><span class="sxs-lookup"><span data-stu-id="d05a0-131">hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands that are equivalent toohello above are:</span></span>
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-hello-built-in-php-configurations"></a><span data-ttu-id="d05a0-132">Så här: ändra hello inbyggda PHP-konfigurationer</span><span class="sxs-lookup"><span data-stu-id="d05a0-132">How to: Change hello built-in PHP configurations</span></span>
<span data-ttu-id="d05a0-133">Du kan ändra någon av hello konfigurationsalternativ genom att följa hello stegen nedan för alla inbyggda PHP-körning.</span><span class="sxs-lookup"><span data-stu-id="d05a0-133">For any built-in PHP runtime, you can change any of hello configuration options by following hello steps below.</span></span> <span data-ttu-id="d05a0-134">(Mer information om php.ini-direktiven finns [förteckning över php.ini-direktiv].)</span><span class="sxs-lookup"><span data-stu-id="d05a0-134">(For information about php.ini directives, see [List of php.ini directives].)</span></span>

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a><span data-ttu-id="d05a0-135">Ändra PHP\_INI\_användare, PHP\_INI\_PERDIR, PHP\_INI\_alla konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="d05a0-135">Changing PHP\_INI\_USER, PHP\_INI\_PERDIR, PHP\_INI\_ALL configuration settings</span></span>
1. <span data-ttu-id="d05a0-136">Lägg till en [. user.ini] filen tooyour rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="d05a0-136">Add a [.user.ini] file tooyour root directory.</span></span>
2. <span data-ttu-id="d05a0-137">Lägg till konfigurationen inställningar toohello `.user.ini` med hello samma syntax som du vill använda i en `php.ini` fil.</span><span class="sxs-lookup"><span data-stu-id="d05a0-137">Add configuration settings toohello `.user.ini` file using hello same syntax you would use in a `php.ini` file.</span></span> <span data-ttu-id="d05a0-138">Om du vill ha tooturn hello exempelvis `display_errors` inställningen vidare samt anger `upload_max_filesize` too10M, ange din `.user.ini` filen innehåller den här texten:</span><span class="sxs-lookup"><span data-stu-id="d05a0-138">For example, if you wanted tooturn hello `display_errors` setting on and set `upload_max_filesize` setting too10M, your `.user.ini` file would contain this text:</span></span>
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on toowrite errors tood:\home\LogFiles\php_errors.log
        ; log_errors=On
3. <span data-ttu-id="d05a0-139">Distribuera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d05a0-139">Deploy your web app.</span></span>
4. <span data-ttu-id="d05a0-140">Starta om hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d05a0-140">Restart hello web app.</span></span> <span data-ttu-id="d05a0-141">(Omstart krävs eftersom hello frekvens med vilken PHP läser `.user.ini` filer styrs av hello `user_ini.cache_ttl` som är en inställning av system och 300 sekunder (fem minuter) som standard.</span><span class="sxs-lookup"><span data-stu-id="d05a0-141">(Restarting is necessary because hello frequency with which PHP reads `.user.ini` files is governed by hello `user_ini.cache_ttl` setting, which is a system level setting and is 300 seconds (5 minutes) by default.</span></span> <span data-ttu-id="d05a0-142">Starta om webbprogrammet för hello tvingar tooread hello nya inställningar för PHP i hello `.user.ini` filen.)</span><span class="sxs-lookup"><span data-stu-id="d05a0-142">Restarting hello web app forces PHP tooread hello new settings in hello `.user.ini` file.)</span></span>

<span data-ttu-id="d05a0-143">Som ett alternativ toousing en `.user.ini` fil, du kan använda hello [ini_set()] funktion i skript tooset konfigurationsalternativ som inte är på systemnivå direktiven.</span><span class="sxs-lookup"><span data-stu-id="d05a0-143">As an alternative toousing a `.user.ini` file, you can use hello [ini_set()] function in scripts tooset configuration options that are not system-level directives.</span></span>

### <a name="changing-phpinisystem-configuration-settings"></a><span data-ttu-id="d05a0-144">Ändra PHP\_INI\_SYSTEM konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="d05a0-144">Changing PHP\_INI\_SYSTEM configuration settings</span></span>
1. <span data-ttu-id="d05a0-145">Lägga till Appinställningen-tooyour Web App med hello nyckel `PHP_INI_SCAN_DIR` och värde`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="d05a0-145">Add an App Setting tooyour Web App with hello key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
2. <span data-ttu-id="d05a0-146">Skapa en `settings.ini` filen med Kudu-konsolen (http://&lt;site-name&gt;. scm.azurewebsite.net) i hello `d:\home\site\ini` directory.</span><span class="sxs-lookup"><span data-stu-id="d05a0-146">Create an `settings.ini` file using Kudu Console (http://&lt;site-name&gt;.scm.azurewebsite.net) in hello `d:\home\site\ini` directory.</span></span>
3. <span data-ttu-id="d05a0-147">Lägg till konfigurationen inställningar toohello `settings.ini` med hello samma syntax som du vill använda i en php.ini-fil.</span><span class="sxs-lookup"><span data-stu-id="d05a0-147">Add configuration settings toohello `settings.ini` file using hello same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="d05a0-148">Om du vill ha toopoint hello exempelvis `curl.cainfo` inställningen tooa `*.crt` fil och anger ”wincache.maxfilesize' inställningen too512K, din `settings.ini` filen innehåller den här texten:</span><span class="sxs-lookup"><span data-stu-id="d05a0-148">For example, if you wanted toopoint hello `curl.cainfo` setting tooa `*.crt` file and set 'wincache.maxfilesize' setting too512K, your `settings.ini` file would contain this text:</span></span>
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. <span data-ttu-id="d05a0-149">Starta om webbprogrammet tooload hello ändringarna.</span><span class="sxs-lookup"><span data-stu-id="d05a0-149">Restart your Web App tooload hello changes.</span></span>

## <a name="how-to-enable-extensions-in-hello-default-php-runtime"></a><span data-ttu-id="d05a0-150">Så här: aktivera tillägg i hello standard PHP-körning</span><span class="sxs-lookup"><span data-stu-id="d05a0-150">How to: Enable extensions in hello default PHP runtime</span></span>
<span data-ttu-id="d05a0-151">Enligt beskrivningen i hello föregående avsnitt, hello bästa sätt toosee hello PHP standardversionen, dess standardkonfigurationen och hello aktiverat tillägg är toodeploy ett skript som anropar [phpinfo()].</span><span class="sxs-lookup"><span data-stu-id="d05a0-151">As noted in hello previous section, hello best way toosee hello default PHP version, its default configuration, and hello enabled extensions is toodeploy a script that calls [phpinfo()].</span></span> <span data-ttu-id="d05a0-152">tooenable ytterligare tillägg, så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="d05a0-152">tooenable additional extensions, follow hello steps below:</span></span>

### <a name="configure-via-ini-settings"></a><span data-ttu-id="d05a0-153">Konfigurera via ini-inställningar</span><span class="sxs-lookup"><span data-stu-id="d05a0-153">Configure via ini settings</span></span>
1. <span data-ttu-id="d05a0-154">Lägg till en `ext` directory toohello `d:\home\site` directory.</span><span class="sxs-lookup"><span data-stu-id="d05a0-154">Add a `ext` directory toohello `d:\home\site` directory.</span></span>
2. <span data-ttu-id="d05a0-155">Placera `.dll` tilläggsfiler i hello `ext` katalog (till exempel `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="d05a0-155">Put `.dll` extension files in hello `ext` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="d05a0-156">Kontrollera att hello tillägg är kompatibla med standardversionen av PHP och är VC9 och icke-trådsäkra (nts)-kompatibel.</span><span class="sxs-lookup"><span data-stu-id="d05a0-156">Make sure that hello extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="d05a0-157">Lägga till Appinställningen-tooyour Web App med hello nyckel `PHP_INI_SCAN_DIR` och värde`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="d05a0-157">Add an App Setting tooyour Web App with hello key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
4. <span data-ttu-id="d05a0-158">Skapa en `ini` filen i `d:\home\site\ini` kallas `extensions.ini`.</span><span class="sxs-lookup"><span data-stu-id="d05a0-158">Create an `ini` file in `d:\home\site\ini` called `extensions.ini`.</span></span>
5. <span data-ttu-id="d05a0-159">Lägg till konfigurationen inställningar toohello `extensions.ini` med hello samma syntax som du vill använda i en php.ini-fil.</span><span class="sxs-lookup"><span data-stu-id="d05a0-159">Add configuration settings toohello `extensions.ini` file using hello same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="d05a0-160">Om du vill ha tooenable hello-MongoDB och XDebug tillägg, till exempel din `extensions.ini` filen innehåller den här texten:</span><span class="sxs-lookup"><span data-stu-id="d05a0-160">For example, if you wanted tooenable hello MongoDB and XDebug extensions, your `extensions.ini` file would contain this text:</span></span>
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. <span data-ttu-id="d05a0-161">Starta om webbprogrammet tooload hello ändringarna.</span><span class="sxs-lookup"><span data-stu-id="d05a0-161">Restart your Web App tooload hello changes.</span></span>

### <a name="configure-via-app-setting"></a><span data-ttu-id="d05a0-162">Konfigurera via Appinställningen</span><span class="sxs-lookup"><span data-stu-id="d05a0-162">Configure via App Setting</span></span>
1. <span data-ttu-id="d05a0-163">Lägg till en `bin` directory toohello rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="d05a0-163">Add a `bin` directory toohello root directory.</span></span>
2. <span data-ttu-id="d05a0-164">Placera `.dll` tilläggsfiler i hello `bin` katalog (till exempel `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="d05a0-164">Put `.dll` extension files in hello `bin` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="d05a0-165">Kontrollera att hello tillägg är kompatibla med standardversionen av PHP och är VC9 och icke-trådsäkra (nts)-kompatibel.</span><span class="sxs-lookup"><span data-stu-id="d05a0-165">Make sure that hello extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="d05a0-166">Distribuera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d05a0-166">Deploy your web app.</span></span>
4. <span data-ttu-id="d05a0-167">Bläddra tooyour webbprogram i hello Azure-portalen och klicka på hello **inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="d05a0-167">Browse tooyour web app in hello Azure Portal and click on hello **Settings** button.</span></span>
   
    ![Webbprograminställningarna][settings-button]
5. <span data-ttu-id="d05a0-169">Från hello **inställningar** bladet välj **programinställningar** och rulla toohello **appinställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d05a0-169">From hello **Settings** blade select **Application Settings** and scroll toohello **App settings** section.</span></span>
6. <span data-ttu-id="d05a0-170">I hello **appinställningar** avsnittet, skapa en **PHP_EXTENSIONS** nyckel.</span><span class="sxs-lookup"><span data-stu-id="d05a0-170">In hello **App settings** section, create a **PHP_EXTENSIONS** key.</span></span> <span data-ttu-id="d05a0-171">hello-värdet för den här nyckeln skulle vara en sökväg relativa toowebsite rot: **bin\your ext fil**.</span><span class="sxs-lookup"><span data-stu-id="d05a0-171">hello value for this key would be a path relative toowebsite root: **bin\your-ext-file**.</span></span>
   
    ![Aktivera tillägget på app-inställningar][php-extensions]
7. <span data-ttu-id="d05a0-173">Klicka på hello **spara** knappen hello överst i hello **Web app-inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="d05a0-173">Click hello **Save** button at hello top of hello **Web app settings** blade.</span></span>
   
    ![Spara inställningar][save-button]

<span data-ttu-id="d05a0-175">Zend tillägg stöds också med hjälp av en **PHP_ZENDEXTENSIONS** nyckel.</span><span class="sxs-lookup"><span data-stu-id="d05a0-175">Zend extensions are also supported by using a **PHP_ZENDEXTENSIONS** key.</span></span> <span data-ttu-id="d05a0-176">tooenable flera tillägg inkluderar en kommaavgränsad lista över `.dll` filer för hello app värdet.</span><span class="sxs-lookup"><span data-stu-id="d05a0-176">tooenable multiple extensions, include a comma-separated list of `.dll` files for hello app setting value.</span></span>

## <a name="how-to-use-a-custom-php-runtime"></a><span data-ttu-id="d05a0-177">Så här: använda en anpassad PHP-körning</span><span class="sxs-lookup"><span data-stu-id="d05a0-177">How to: Use a custom PHP runtime</span></span>
<span data-ttu-id="d05a0-178">I stället hello standard PHP-körning kan App Service Web Apps använda en PHP-körning som du anger tooexecute PHP-skript.</span><span class="sxs-lookup"><span data-stu-id="d05a0-178">Instead of hello default PHP runtime, App Service Web Apps can use a PHP runtime that you provide tooexecute PHP scripts.</span></span> <span data-ttu-id="d05a0-179">hello runtime som du anger kan konfigureras med en `php.ini` -fil som du också ange.</span><span class="sxs-lookup"><span data-stu-id="d05a0-179">hello runtime that you provide can be configured by a `php.ini` file that you also provide.</span></span> <span data-ttu-id="d05a0-180">toouse anpassade PHP-körning med Web Apps åtgärderna hello nedan.</span><span class="sxs-lookup"><span data-stu-id="d05a0-180">toouse a custom PHP runtime with Web Apps, follow hello steps below.</span></span>

1. <span data-ttu-id="d05a0-181">Hämta en icke-trådsäkra, VC9 eller VC11 kompatibel version av PHP för Windows.</span><span class="sxs-lookup"><span data-stu-id="d05a0-181">Obtain a non-thread-safe, VC9 or VC11 compatible version of PHP for Windows.</span></span> <span data-ttu-id="d05a0-182">Nya versioner av PHP för Windows finns här: [http://windows.php.net/download/].</span><span class="sxs-lookup"><span data-stu-id="d05a0-182">Recent releases of PHP for Windows can be found here: [http://windows.php.net/download/].</span></span> <span data-ttu-id="d05a0-183">Äldre versioner kan hittas i här hello-Arkiv: [http://windows.php.net/downloads/releases/archives/].</span><span class="sxs-lookup"><span data-stu-id="d05a0-183">Older releases can be found in hello archive here: [http://windows.php.net/downloads/releases/archives/].</span></span>
2. <span data-ttu-id="d05a0-184">Ändra hello `php.ini` filen för din runtime.</span><span class="sxs-lookup"><span data-stu-id="d05a0-184">Modify hello `php.ini` file for your runtime.</span></span> <span data-ttu-id="d05a0-185">Observera att alla inställningar som är endast system-nivå direktiv ignoreras av Web Apps.</span><span class="sxs-lookup"><span data-stu-id="d05a0-185">Note that any configuration settings that are system-level-only directives will be ignored by Web Apps.</span></span> <span data-ttu-id="d05a0-186">(Mer information om endast system-nivå direktiven finns [förteckning över php.ini-direktiv]).</span><span class="sxs-lookup"><span data-stu-id="d05a0-186">(For information about system-level-only directives, see [List of php.ini directives]).</span></span>
3. <span data-ttu-id="d05a0-187">Du kan också lägga till tillägg tooyour PHP-körning och aktivera dem i hello `php.ini` fil.</span><span class="sxs-lookup"><span data-stu-id="d05a0-187">Optionally, add extensions tooyour PHP runtime and enable them in hello `php.ini` file.</span></span>
4. <span data-ttu-id="d05a0-188">Lägg till en `bin` directory tooyour rotkatalog och placera hello katalogen där PHP-körning i den (till exempel `bin\php`).</span><span class="sxs-lookup"><span data-stu-id="d05a0-188">Add a `bin` directory tooyour root directory, and put hello directory that contains your PHP runtime in it (for example, `bin\php`).</span></span>
5. <span data-ttu-id="d05a0-189">Distribuera ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d05a0-189">Deploy your web app.</span></span>
6. <span data-ttu-id="d05a0-190">Bläddra tooyour webbprogram i hello Azure-portalen och klicka på hello **inställningar** knappen.</span><span class="sxs-lookup"><span data-stu-id="d05a0-190">Browse tooyour web app in hello Azure Portal and click on hello **Settings** button.</span></span>
   
    ![Webbprograminställningarna][settings-button]
7. <span data-ttu-id="d05a0-192">Från hello **inställningar** bladet välj **programinställningar** och rulla toohello **Hanterarmappningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d05a0-192">From hello **Settings** blade select **Application Settings** and scroll toohello **Handler mappings** section.</span></span> <span data-ttu-id="d05a0-193">Lägg till `*.php` toohello tillägget fältet och Lägg till hello sökvägen toohello `php-cgi.exe` körbara.</span><span class="sxs-lookup"><span data-stu-id="d05a0-193">Add `*.php` toohello Extension field and add hello path toohello `php-cgi.exe` executable.</span></span> <span data-ttu-id="d05a0-194">Om du placerar PHP-körning i hello `bin` katalog i hello roten av du programmet hello åtkomstsökvägen `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span><span class="sxs-lookup"><span data-stu-id="d05a0-194">If you put your PHP runtime in hello `bin` directory in hello root of you application, hello path will be `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span></span>
   
    ![Ange hanterare i hanterare mappningar][handler-mappings]
8. <span data-ttu-id="d05a0-196">Klicka på hello **spara** knappen hello överst i hello **Web app-inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="d05a0-196">Click hello **Save** button at hello top of hello **Web app settings** blade.</span></span>
   
    ![Spara inställningar][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a><span data-ttu-id="d05a0-198">Så här: aktivera Composer automatisering i Azure</span><span class="sxs-lookup"><span data-stu-id="d05a0-198">How to: Enable Composer automation in Azure</span></span>
<span data-ttu-id="d05a0-199">Som standard göra Apptjänst inte något med composer.json, om du har en i PHP-projektet.</span><span class="sxs-lookup"><span data-stu-id="d05a0-199">By default, App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="d05a0-200">Om du använder [Git-distribution](app-service-deploy-local-git.md), kan du aktivera composer.json bearbetning under `git push` genom att aktivera hello Composer tillägg.</span><span class="sxs-lookup"><span data-stu-id="d05a0-200">If you use [Git deployment](app-service-deploy-local-git.md), you can enable composer.json processing during `git push` by enabling hello Composer extension.</span></span>

> [!NOTE]
> <span data-ttu-id="d05a0-201">Du kan [rösta på förstklassigt Composer stöd i App Service här](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span><span class="sxs-lookup"><span data-stu-id="d05a0-201">You can [vote for first-class Composer support in App Service here](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span></span>
> 
> 

1. <span data-ttu-id="d05a0-202">I din PHP web Apps bladet i hello [Azure-portalen](https://portal.azure.com), klickar du på **verktyg** > **tillägg**.</span><span class="sxs-lookup"><span data-stu-id="d05a0-202">In your PHP web app's blade in hello [Azure portal](https://portal.azure.com), click **Tools** > **Extensions**.</span></span>
   
    ![Azure Portal inställningar bladet tooenable Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-settings.png)
2. <span data-ttu-id="d05a0-204">Klicka på **Lägg till**, klicka på **Composer**.</span><span class="sxs-lookup"><span data-stu-id="d05a0-204">Click **Add**, then click **Composer**.</span></span>
   
    ![Lägg till Composer tillägget tooenable Composer automation i Azure](./media/web-sites-php-configure/composer-extension-add.png)
3. <span data-ttu-id="d05a0-206">Klicka på **OK** tooaccept juridiska villkor.</span><span class="sxs-lookup"><span data-stu-id="d05a0-206">Click **OK** tooaccept legal terms.</span></span> <span data-ttu-id="d05a0-207">Klicka på **OK** igen tooadd hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="d05a0-207">Click **OK** again tooadd hello extension.</span></span>
   
    <span data-ttu-id="d05a0-208">Hej **installerat tillägg** bladet visas nu hello Composer tillägg.</span><span class="sxs-lookup"><span data-stu-id="d05a0-208">hello **Installed extensions** blade will now show hello Composer extension.</span></span>  
    <span data-ttu-id="d05a0-209">![Acceptera juridiska villkor tooenable Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-view.png)</span><span class="sxs-lookup"><span data-stu-id="d05a0-209">![Accept legal terms tooenable Composer automation in Azure](./media/web-sites-php-configure/composer-extension-view.png)</span></span>
4. <span data-ttu-id="d05a0-210">Nu kan utföra `git add`, `git commit`, och `git push` precis som i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d05a0-210">Now, perform `git add`, `git commit`, and `git push` like in hello previous section.</span></span> <span data-ttu-id="d05a0-211">Du ser nu att installeras Composer beroenden som definierats i composer.json.</span><span class="sxs-lookup"><span data-stu-id="d05a0-211">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Git-distribution med Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a><span data-ttu-id="d05a0-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d05a0-213">Next steps</span></span>
<span data-ttu-id="d05a0-214">Mer information finns i hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="d05a0-214">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

> [!NOTE]
> <span data-ttu-id="d05a0-215">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="d05a0-215">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="d05a0-216">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="d05a0-216">No credit cards required; no commitments.</span></span>
> 
> 

[kostnadsfri utvärderingsversion]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[förteckning över php.ini-direktiv]: http://www.php.net/manual/en/ini.list.php
[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: http://www.php.net/manual/en/function.ini-set.php
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
[http://windows.php.net/download/]: http://windows.php.net/download/
[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png

