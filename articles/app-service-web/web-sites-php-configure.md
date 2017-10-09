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
# <a name="configure-php-in-azure-app-service-web-apps"></a>Så här konfigurerar du PHP i Azure App Service Web Apps
## <a name="introduction"></a>Introduktion
Den här guiden visar hur tooconfigure hello inbyggda PHP-körning för Web Apps i [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)anger en anpassad PHP-körning och aktivera tillägg. toouse Apptjänst, registrera dig för hello [kostnadsfri utvärderingsversion]. tooget hello mest från den här guiden du först skapa en PHP-webbapp i App Service.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-hello-built-in-php-version"></a>Så här: ändra hello inbyggda PHP-version
Som standard är PHP 5.5 installerat och omedelbart tillgängliga för användning när du skapar en webbapp i Apptjänst. Hej bästa sätt toosee hello tillgängliga versionen revision, standardkonfigurationen och hello aktiverade tillägg är toodeploy ett skript som anropar hello [phpinfo()] funktion.

PHP 5.6 och PHP 7.0 versioner är också tillgängliga, men inte aktiverad som standard. tooupdate hello PHP-version, Följ någon av följande metoder:

### <a name="azure-portal"></a>Azure Portal
1. Bläddra tooyour webbprogram i hello [Azure Portal](https://portal.azure.com) och klicka på hello **inställningar** knappen.
   
    ![Webbprograminställningarna][settings-button]
2. Från hello **inställningar** bladet välj **programinställningar** och välj hello nya PHP-versionen.
   
    ![Programinställningar][application-settings]
3. Klicka på hello **spara** knappen hello överst i hello **Web app-inställningar** bladet.
   
    ![Spara inställningar][save-button]

### <a name="azure-powershell-windows"></a>Azure PowerShell (Windows)
1. Öppna Azure PowerShell och tooyour inloggningskonto:
   
        PS C:\> Login-AzureRmAccount
2. Ange hello PHP-version för hello webbprogrammet.
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. hello PHP-version anges nu. Du kan bekräfta inställningarna:
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a>Azure-kommandoradsgränssnittet (Linux, Mac, Windows)
toouse hello Azure kommandoradsgränssnitt måste du ha **Node.js** installerat på datorn.

1. Öppna Terminal och tooyour inloggningskonto.
   
        azure login
2. Ange hello PHP-version för hello webbprogrammet.
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. hello PHP-version anges nu. Du kan bekräfta inställningarna:
   
        azure site show {app-name}

> [!NOTE] 
> Hej [Azure CLI 2.0](https://github.com/Azure/azure-cli) kommandon som är likvärdiga toohello ovan är:
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-hello-built-in-php-configurations"></a>Så här: ändra hello inbyggda PHP-konfigurationer
Du kan ändra någon av hello konfigurationsalternativ genom att följa hello stegen nedan för alla inbyggda PHP-körning. (Mer information om php.ini-direktiven finns [förteckning över php.ini-direktiv].)

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a>Ändra PHP\_INI\_användare, PHP\_INI\_PERDIR, PHP\_INI\_alla konfigurationsinställningar
1. Lägg till en [. user.ini] filen tooyour rotkatalog.
2. Lägg till konfigurationen inställningar toohello `.user.ini` med hello samma syntax som du vill använda i en `php.ini` fil. Om du vill ha tooturn hello exempelvis `display_errors` inställningen vidare samt anger `upload_max_filesize` too10M, ange din `.user.ini` filen innehåller den här texten:
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on toowrite errors tood:\home\LogFiles\php_errors.log
        ; log_errors=On
3. Distribuera ditt webbprogram.
4. Starta om hello webbprogram. (Omstart krävs eftersom hello frekvens med vilken PHP läser `.user.ini` filer styrs av hello `user_ini.cache_ttl` som är en inställning av system och 300 sekunder (fem minuter) som standard. Starta om webbprogrammet för hello tvingar tooread hello nya inställningar för PHP i hello `.user.ini` filen.)

Som ett alternativ toousing en `.user.ini` fil, du kan använda hello [ini_set()] funktion i skript tooset konfigurationsalternativ som inte är på systemnivå direktiven.

### <a name="changing-phpinisystem-configuration-settings"></a>Ändra PHP\_INI\_SYSTEM konfigurationsinställningar
1. Lägga till Appinställningen-tooyour Web App med hello nyckel `PHP_INI_SCAN_DIR` och värde`d:\home\site\ini`
2. Skapa en `settings.ini` filen med Kudu-konsolen (http://&lt;site-name&gt;. scm.azurewebsite.net) i hello `d:\home\site\ini` directory.
3. Lägg till konfigurationen inställningar toohello `settings.ini` med hello samma syntax som du vill använda i en php.ini-fil. Om du vill ha toopoint hello exempelvis `curl.cainfo` inställningen tooa `*.crt` fil och anger ”wincache.maxfilesize' inställningen too512K, din `settings.ini` filen innehåller den här texten:
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. Starta om webbprogrammet tooload hello ändringarna.

## <a name="how-to-enable-extensions-in-hello-default-php-runtime"></a>Så här: aktivera tillägg i hello standard PHP-körning
Enligt beskrivningen i hello föregående avsnitt, hello bästa sätt toosee hello PHP standardversionen, dess standardkonfigurationen och hello aktiverat tillägg är toodeploy ett skript som anropar [phpinfo()]. tooenable ytterligare tillägg, så hello nedan:

### <a name="configure-via-ini-settings"></a>Konfigurera via ini-inställningar
1. Lägg till en `ext` directory toohello `d:\home\site` directory.
2. Placera `.dll` tilläggsfiler i hello `ext` katalog (till exempel `php_xdebug.dll`). Kontrollera att hello tillägg är kompatibla med standardversionen av PHP och är VC9 och icke-trådsäkra (nts)-kompatibel.
3. Lägga till Appinställningen-tooyour Web App med hello nyckel `PHP_INI_SCAN_DIR` och värde`d:\home\site\ini`
4. Skapa en `ini` filen i `d:\home\site\ini` kallas `extensions.ini`.
5. Lägg till konfigurationen inställningar toohello `extensions.ini` med hello samma syntax som du vill använda i en php.ini-fil. Om du vill ha tooenable hello-MongoDB och XDebug tillägg, till exempel din `extensions.ini` filen innehåller den här texten:
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. Starta om webbprogrammet tooload hello ändringarna.

### <a name="configure-via-app-setting"></a>Konfigurera via Appinställningen
1. Lägg till en `bin` directory toohello rotkatalog.
2. Placera `.dll` tilläggsfiler i hello `bin` katalog (till exempel `php_xdebug.dll`). Kontrollera att hello tillägg är kompatibla med standardversionen av PHP och är VC9 och icke-trådsäkra (nts)-kompatibel.
3. Distribuera ditt webbprogram.
4. Bläddra tooyour webbprogram i hello Azure-portalen och klicka på hello **inställningar** knappen.
   
    ![Webbprograminställningarna][settings-button]
5. Från hello **inställningar** bladet välj **programinställningar** och rulla toohello **appinställningar** avsnitt.
6. I hello **appinställningar** avsnittet, skapa en **PHP_EXTENSIONS** nyckel. hello-värdet för den här nyckeln skulle vara en sökväg relativa toowebsite rot: **bin\your ext fil**.
   
    ![Aktivera tillägget på app-inställningar][php-extensions]
7. Klicka på hello **spara** knappen hello överst i hello **Web app-inställningar** bladet.
   
    ![Spara inställningar][save-button]

Zend tillägg stöds också med hjälp av en **PHP_ZENDEXTENSIONS** nyckel. tooenable flera tillägg inkluderar en kommaavgränsad lista över `.dll` filer för hello app värdet.

## <a name="how-to-use-a-custom-php-runtime"></a>Så här: använda en anpassad PHP-körning
I stället hello standard PHP-körning kan App Service Web Apps använda en PHP-körning som du anger tooexecute PHP-skript. hello runtime som du anger kan konfigureras med en `php.ini` -fil som du också ange. toouse anpassade PHP-körning med Web Apps åtgärderna hello nedan.

1. Hämta en icke-trådsäkra, VC9 eller VC11 kompatibel version av PHP för Windows. Nya versioner av PHP för Windows finns här: [http://windows.php.net/download/]. Äldre versioner kan hittas i här hello-Arkiv: [http://windows.php.net/downloads/releases/archives/].
2. Ändra hello `php.ini` filen för din runtime. Observera att alla inställningar som är endast system-nivå direktiv ignoreras av Web Apps. (Mer information om endast system-nivå direktiven finns [förteckning över php.ini-direktiv]).
3. Du kan också lägga till tillägg tooyour PHP-körning och aktivera dem i hello `php.ini` fil.
4. Lägg till en `bin` directory tooyour rotkatalog och placera hello katalogen där PHP-körning i den (till exempel `bin\php`).
5. Distribuera ditt webbprogram.
6. Bläddra tooyour webbprogram i hello Azure-portalen och klicka på hello **inställningar** knappen.
   
    ![Webbprograminställningarna][settings-button]
7. Från hello **inställningar** bladet välj **programinställningar** och rulla toohello **Hanterarmappningar** avsnitt. Lägg till `*.php` toohello tillägget fältet och Lägg till hello sökvägen toohello `php-cgi.exe` körbara. Om du placerar PHP-körning i hello `bin` katalog i hello roten av du programmet hello åtkomstsökvägen `D:\home\site\wwwroot\bin\php\php-cgi.exe`.
   
    ![Ange hanterare i hanterare mappningar][handler-mappings]
8. Klicka på hello **spara** knappen hello överst i hello **Web app-inställningar** bladet.
   
    ![Spara inställningar][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Så här: aktivera Composer automatisering i Azure
Som standard göra Apptjänst inte något med composer.json, om du har en i PHP-projektet. Om du använder [Git-distribution](app-service-deploy-local-git.md), kan du aktivera composer.json bearbetning under `git push` genom att aktivera hello Composer tillägg.

> [!NOTE]
> Du kan [rösta på förstklassigt Composer stöd i App Service här](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!
> 
> 

1. I din PHP web Apps bladet i hello [Azure-portalen](https://portal.azure.com), klickar du på **verktyg** > **tillägg**.
   
    ![Azure Portal inställningar bladet tooenable Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-settings.png)
2. Klicka på **Lägg till**, klicka på **Composer**.
   
    ![Lägg till Composer tillägget tooenable Composer automation i Azure](./media/web-sites-php-configure/composer-extension-add.png)
3. Klicka på **OK** tooaccept juridiska villkor. Klicka på **OK** igen tooadd hello-tillägget.
   
    Hej **installerat tillägg** bladet visas nu hello Composer tillägg.  
    ![Acceptera juridiska villkor tooenable Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-view.png)
4. Nu kan utföra `git add`, `git commit`, och `git push` precis som i hello föregående avsnitt. Du ser nu att installeras Composer beroenden som definierats i composer.json.
   
    ![Git-distribution med Composer automatisering i Azure](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [PHP Developer Center](/develop/php/).

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
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

