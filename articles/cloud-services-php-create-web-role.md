---
title: "aaaCreate Azure webb- och arbetsroller roller för PHP | Microsoft Docs"
description: "En guide toocreating PHP webb- och arbetsroller roller i en Azure-molntjänst och konfigurera hello PHP-körning."
services: 
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9f7ccda0-bd96-4f7b-a7af-fb279a9e975b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 04a6e8c9c379cb0f854645941b6bc7d614bb91f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-php-web-and-worker-roles"></a>Hur toocreate PHP webb- och arbetsroller roller
## <a name="overview"></a>Översikt
Den här guiden visar hur toocreate PHP webb eller arbetare roller i en Windows-utvecklingsmiljö väljer en viss version av PHP från hello ”inbyggd” versioner som är tillgängliga, ändra hello PHP-konfiguration, aktivera tillägg och slutligen distribuera tooAzure. Här beskrivs också hur tooconfigure en webb- eller arbetarroll roll-toouse en PHP-körning (med anpassad konfiguration och tillägg) som du anger.

## <a name="what-are-php-web-and-worker-roles"></a>Vad är PHP webb- och arbetsroller roller?
Azure tillhandahåller tre beräkningsmodeller för program som körs: Azure App Service, Azure virtuella datorer och Azure Cloud Services. Alla tre modeller stöder PHP. Molntjänster som innehåller webb-och arbetsroller innehåller *plattform som en tjänst (PaaS)*. I en molntjänst tillhandahåller en webbroll en dedikerad webbplats i Internet Information Services (IIS) server toohost frontend-webbprogram. En arbetsroll kan köra asynkrona, tidskrävande eller beständiga uppgifter oberoende av användarinteraktion eller indata.

Mer information om dessa alternativ finns [beräkning som värd för alternativen i Azure](cloud-services/cloud-services-choose-me.md).

## <a name="download-hello-azure-sdk-for-php"></a>Hämta hello Azure SDK för PHP
Hej [Azure SDK för PHP] består av flera komponenter. Den här artikeln använder två av dem: Azure PowerShell och hello Azure emulatorerna. Dessa två komponenter kan installeras via hello Microsoft Web Platform Installer. Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a name="create-a-cloud-services-project"></a>Skapa ett projekt med molntjänster
hello första steget i att skapa en PHP-webbplats eller arbetsprocess roll är toocreate Azure Service-projekt. Azure Service-projekt som fungerar som en logisk behållare för webb-och arbetsroller och hello projektet innehåller [service definition (.csdef)] och [tjänstkonfiguration (.cscfg)] filer.

toocreate ett nytt projekt i Azure Service köra Azure PowerShell som administratör och kör följande kommando hello:

    PS C:\>New-AzureServiceProject myProject

Det här kommandot skapar en ny katalog (`myProject`) toowhich som du kan lägga till webb-och arbetsroller.

## <a name="add-php-web-or-worker-roles"></a>Lägga till PHP-webbplats eller arbetsprocess roller
tooadd en PHP tooa webbrollsprojektet, kör följande kommando från i hello projektets rotkatalog hello:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Använd följande kommando för en arbetsroll:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> Hej `roleName` parametern är valfri. Om det utelämnas ska hello rollnamn skapas automatiskt. hello första webbroll skapat kommer att `WebRole1`, hello andra kommer att `WebRole2`och så vidare. hello första arbetsrollen skapat kommer att `WorkerRole1`, hello andra kommer att `WorkerRole2`och så vidare.
>
>

## <a name="specify-hello-built-in-php-version"></a>Ange hello inbyggda PHP-version
När du lägger till ett PHP webb eller arbetare tooa rollprojekt ändras hello projektet konfigurationsfiler så att PHP kommer att installeras på varje webb eller arbetare instans av programmet när det distribueras. toosee hello versionen av PHP som installeras som standard kör hello följande kommando:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

hello utdata från kommandot hello ser ut ungefär toowhat visas nedan. I det här exemplet hello `IsDefault` flaggan anges för`true` för PHP 5.3.17, som anger att det blir hello PHP standardversionen installerad.

```
Runtime Version     PackageUri                      IsDefault
------- -------     ----------                      ---------
Node 0.6.17         http://nodertncu.blob.core...   False
Node 0.6.20         http://nodertncu.blob.core...   True
Node 0.8.4          http://nodertncu.blob.core...   False
IISNode 0.1.21      http://nodertncu.blob.core...   True
Cache 1.8.0         http://nodertncu.blob.core...   True
PHP 5.3.17          http://nodertncu.blob.core...   True
PHP 5.4.0           http://nodertncu.blob.core...   False
```

Du kan ange hello PHP runtime version tooany av hello PHP-versionerna. Till exempel tooset hello PHP-version (för en roll med namnet hello `roleName`) too5.4.0, Använd hello följande kommando:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> Tillgängliga PHP-versioner kan ändras i framtida hello.
>
>

## <a name="customize-hello-built-in-php-runtime"></a>Anpassa hello inbyggda PHP-körning
Du har fullständig kontroll över hello konfigurationen av hello PHP-körning som installeras när du gör hello ovan, även ändringar av `php.ini` inställningar och aktivering av tillägg.

toocustomize Hej inbyggda PHP-körning, gör du följande:

1. Lägg till en ny mapp med namnet `php`, toohello `bin` katalogen för webbrollen. För en arbetsroll lägga till den toohello roll rotkatalog.
2. I hello `php` mapp, skapa en annan mapp med namnet `ext`. Placera alla `.dll` tilläggsfiler (t.ex. `php_mongo.dll`) som du vill tooenable i den här mappen.
3. Lägg till en `php.ini` filen toohello `php` mapp. Aktivera alla anpassade tillägg och ange en PHP-direktiv i filen. Om du vill ha tooturn exempelvis `display_errors` på och aktivera hello `php_mongo.dll` tillägg, hello innehållet i din `php.ini` filen blir som följer:

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> Alla inställningar som du inte uttryckligen anger i hello `php.ini` fil att du anger kommer automatiskt att ange standardvärden för tootheir. Tänk dock på att du kan lägga till en fullständig `php.ini` fil.
>
>

## <a name="use-your-own-php-runtime"></a>Använda din egen PHP-körning
I vissa fall kan i stället för att välja en inbyggd PHP-körning och konfigurera det som beskrivs ovan, kan du tooprovide egna PHP-körning. Du kan till exempel använda hello samma PHP-körning i en webb- eller arbetarroll roll som du använder i din utvecklingsmiljö. Detta gör det enklare tooensure att hello program inte att ändra beteende i din produktionsmiljö.

### <a name="configure-a-web-role-toouse-your-own-php-runtime"></a>Konfigurera en webbserver-rollen toouse egna PHP-körning
tooconfigure en webbserver-rollen toouse en PHP-körning som du anger så här:

1. Skapa ett Azure Service-projekt och Lägg till en PHP-webbroll enligt beskrivningen tidigare i det här avsnittet.
2. Skapa en `php` mapp i hello `bin` mapp som finns i rotkatalogen för din webbserver-rollen och Lägg sedan till din PHP-körning (alla binärfiler, konfigurationsfiler, undermappar, etc.) toohello `php` mapp.
3. (VALFRITT) Om PHP-körning använder hello [Drivers Microsoft för PHP för SQL Server][sqlsrv drivers], behöver du tooconfigure din webbserver-rollen tooinstall [SQL Server Native Client 2012] [ sql native client] när den har etablerats. toodo detta, Lägg till hello [sqlncli.msi x64 installer] toohello `bin` mapp i rotkatalogen för din webbroll. hello startskript som beskrivs i nästa steg i hello körs tyst hello installer när hello rollen etableras. Om PHP-körning inte använder hello Microsoft Drivers för PHP för SQL Server, kan du ta bort hello följande rad från hello-skript som visas i nästa steg i hello:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Definiera en startåtgärd som konfigurerar [Internet Information Services (IIS)] [ iis.net] toouse din PHP runtime toohandle begäranden om `.php` sidor. toodo detta, öppna hello `setup_web.cmd` fil (i hello `bin` filen i rotkatalogen för din webbroll) i en textredigerare och Ersätt innehållet med hello följande skript:

    ```cmd
    @ECHO ON
    cd "%~dp0"

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
    SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"
    ```
5. Lägg till ditt program filer tooyour webbroll rotkatalog. Detta kan vara hello webbserverns rotkatalog.
6. Publicera programmet enligt beskrivningen i hello [publicera programmet](#publish-your-application) nedan.

> [!NOTE]
> Hej `download.ps1` skript (i hello `bin` mappen hello webbroll rotkatalogen) kan tas bort när du har följt hello stegen ovan för att använda din egen PHP-körning.
>
>

### <a name="configure-a-worker-role-toouse-your-own-php-runtime"></a>Konfigurera en worker-rollen toouse egna PHP-körning
tooconfigure en worker-rollen toouse en PHP-körning som du anger så här:

1. Skapa ett Azure Service-projekt och Lägg till en PHP-arbetsroll som beskrivs tidigare i det här avsnittet.
2. Skapa en `php` mapp i rotkatalogen för hello worker rollen och Lägg sedan till din PHP-körning (alla binärfiler, konfigurationsfiler, undermappar, etc.) toohello `php` mapp.
3. (VALFRITT) Om PHP-körning använder [Drivers Microsoft för PHP för SQL Server][sqlsrv drivers], behöver du tooconfigure din worker-rollen tooinstall [SQL Server Native Client 2012] [ sql native client] när den har etablerats. toodo detta, Lägg till hello [sqlncli.msi x64 installer] toohello worker rollen rotkatalog. hello startskript som beskrivs i nästa steg i hello körs tyst hello installer när hello rollen etableras. Om PHP-körning inte använder hello Microsoft Drivers för PHP för SQL Server, kan du ta bort hello följande rad från hello-skript som visas i nästa steg i hello:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Definiera en startåtgärd som lägger till din `php.exe` körbara toohello worker rollen PATH-miljövariabeln när hello rollen etableras. toodo detta, öppna hello `setup_worker.cmd` filen (i rotkatalogen för hello worker rollen) i en textredigerare och Ersätt innehållet med hello följande skript:

    ```cmd
    @echo on

    cd "%~dp0"

    echo Granting permissions for Network Service toohello web root directory...
    icacls ..\ /grant "Network Service":(OI)(CI)W
    if %ERRORLEVEL% neq 0 goto error
    echo OK

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    setx Path "%PATH%;%~dp0php" /M

    if %ERRORLEVEL% neq 0 goto error

    echo SUCCESS
    exit /b 0

    :error

    echo FAILED
    exit /b -1
    ```
5. Lägg till rotkatalogen för din ansökan filer tooyour worker roll.
6. Publicera programmet enligt beskrivningen i hello [publicera programmet](#publish-your-application) nedan.

## <a name="run-your-application-in-hello-compute-and-storage-emulators"></a>Kör ditt program i hello emulatorerna beräkning och lagring
hello ange Azure emulatorerna en lokal miljö där du kan testa din Azure-program innan du distribuerar toohello moln. Det finns några skillnader mellan hello emulatorerna och hello Azure-miljön. toounderstand detta bättre, se [Använd hello Azure storage-emulatorn för utveckling och testning](storage/common/storage-use-emulator.md).

Observera att du måste ha PHP installerat lokalt toouse hello-beräkningsemulatorn. Hej beräkningsemulatorn använder din lokala PHP-installation toorun ditt program.

toorun ditt projekt i hello emulatorerna köra följande kommando från projektets rotkatalog hello:

    PS C:\MyProject> Start-AzureEmulator

Utdata liknande toothis visas:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Du kan se ditt program som körs i hello-emulatorn genom att öppna en webbläsare och surfning toohello lokal adress visas i hello utdata (`http://127.0.0.1:81` i hello exempel på utdata ovan).

toostop hello emulatorerna kör kommandot:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Publicera programmet
toopublish ditt program behöver du toofirst importera dina publiceringsinställningar med hjälp av hello [importera AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet. Sedan kan du publicera dina program med hjälp av hello [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet. Information om att logga in finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [PHP Developer Center](/develop/php/).

[Azure SDK för PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[service definition (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[tjänstkonfiguration (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648
