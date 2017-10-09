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
# <a name="how-toocreate-php-web-and-worker-roles"></a><span data-ttu-id="ed90a-103">Hur toocreate PHP webb- och arbetsroller roller</span><span class="sxs-lookup"><span data-stu-id="ed90a-103">How toocreate PHP web and worker roles</span></span>
## <a name="overview"></a><span data-ttu-id="ed90a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="ed90a-104">Overview</span></span>
<span data-ttu-id="ed90a-105">Den här guiden visar hur toocreate PHP webb eller arbetare roller i en Windows-utvecklingsmiljö väljer en viss version av PHP från hello ”inbyggd” versioner som är tillgängliga, ändra hello PHP-konfiguration, aktivera tillägg och slutligen distribuera tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ed90a-105">This guide will show you how toocreate PHP web or worker roles in a Windows development environment, choose a specific version of PHP from hello "built-in" versions available, change hello PHP configuration, enable extensions, and finally, deploy tooAzure.</span></span> <span data-ttu-id="ed90a-106">Här beskrivs också hur tooconfigure en webb- eller arbetarroll roll-toouse en PHP-körning (med anpassad konfiguration och tillägg) som du anger.</span><span class="sxs-lookup"><span data-stu-id="ed90a-106">It also describes how tooconfigure a web or worker role toouse a PHP runtime (with custom configuration and extensions) that you provide.</span></span>

## <a name="what-are-php-web-and-worker-roles"></a><span data-ttu-id="ed90a-107">Vad är PHP webb- och arbetsroller roller?</span><span class="sxs-lookup"><span data-stu-id="ed90a-107">What are PHP web and worker roles?</span></span>
<span data-ttu-id="ed90a-108">Azure tillhandahåller tre beräkningsmodeller för program som körs: Azure App Service, Azure virtuella datorer och Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="ed90a-108">Azure provides three compute models for running applications: Azure App Service, Azure Virtual Machines, and Azure Cloud Services.</span></span> <span data-ttu-id="ed90a-109">Alla tre modeller stöder PHP.</span><span class="sxs-lookup"><span data-stu-id="ed90a-109">All three models support PHP.</span></span> <span data-ttu-id="ed90a-110">Molntjänster som innehåller webb-och arbetsroller innehåller *plattform som en tjänst (PaaS)*.</span><span class="sxs-lookup"><span data-stu-id="ed90a-110">Cloud Services, which includes web and worker roles, provides *platform as a service (PaaS)*.</span></span> <span data-ttu-id="ed90a-111">I en molntjänst tillhandahåller en webbroll en dedikerad webbplats i Internet Information Services (IIS) server toohost frontend-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="ed90a-111">Within a cloud service, a web role provides a dedicated Internet Information Services (IIS) web server toohost front-end web applications.</span></span> <span data-ttu-id="ed90a-112">En arbetsroll kan köra asynkrona, tidskrävande eller beständiga uppgifter oberoende av användarinteraktion eller indata.</span><span class="sxs-lookup"><span data-stu-id="ed90a-112">A worker role can run asynchronous, long-running or perpetual tasks independent of user interaction or input.</span></span>

<span data-ttu-id="ed90a-113">Mer information om dessa alternativ finns [beräkning som värd för alternativen i Azure](cloud-services/cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="ed90a-113">For more information about these options, see [Compute hosting options provided by Azure](cloud-services/cloud-services-choose-me.md).</span></span>

## <a name="download-hello-azure-sdk-for-php"></a><span data-ttu-id="ed90a-114">Hämta hello Azure SDK för PHP</span><span class="sxs-lookup"><span data-stu-id="ed90a-114">Download hello Azure SDK for PHP</span></span>
<span data-ttu-id="ed90a-115">Hej [Azure SDK för PHP] består av flera komponenter.</span><span class="sxs-lookup"><span data-stu-id="ed90a-115">hello [Azure SDK for PHP] consists of several components.</span></span> <span data-ttu-id="ed90a-116">Den här artikeln använder två av dem: Azure PowerShell och hello Azure emulatorerna.</span><span class="sxs-lookup"><span data-stu-id="ed90a-116">This article will use two of them: Azure PowerShell and hello Azure emulators.</span></span> <span data-ttu-id="ed90a-117">Dessa två komponenter kan installeras via hello Microsoft Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="ed90a-117">These two components can be installed via hello Microsoft Web Platform Installer.</span></span> <span data-ttu-id="ed90a-118">Mer information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ed90a-118">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="create-a-cloud-services-project"></a><span data-ttu-id="ed90a-119">Skapa ett projekt med molntjänster</span><span class="sxs-lookup"><span data-stu-id="ed90a-119">Create a Cloud Services project</span></span>
<span data-ttu-id="ed90a-120">hello första steget i att skapa en PHP-webbplats eller arbetsprocess roll är toocreate Azure Service-projekt.</span><span class="sxs-lookup"><span data-stu-id="ed90a-120">hello first step in creating a PHP web or worker role is toocreate an Azure Service project.</span></span> <span data-ttu-id="ed90a-121">Azure Service-projekt som fungerar som en logisk behållare för webb-och arbetsroller och hello projektet innehåller [service definition (.csdef)] och [tjänstkonfiguration (.cscfg)] filer.</span><span class="sxs-lookup"><span data-stu-id="ed90a-121">an Azure Service project serves as a logical container for web and worker roles, and it contains hello project's [service definition (.csdef)] and [service configuration (.cscfg)] files.</span></span>

<span data-ttu-id="ed90a-122">toocreate ett nytt projekt i Azure Service köra Azure PowerShell som administratör och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ed90a-122">toocreate a new Azure Service project, run Azure PowerShell as an administrator, and execute hello following command:</span></span>

    PS C:\>New-AzureServiceProject myProject

<span data-ttu-id="ed90a-123">Det här kommandot skapar en ny katalog (`myProject`) toowhich som du kan lägga till webb-och arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="ed90a-123">This command will create a new directory (`myProject`) toowhich you can add web and worker roles.</span></span>

## <a name="add-php-web-or-worker-roles"></a><span data-ttu-id="ed90a-124">Lägga till PHP-webbplats eller arbetsprocess roller</span><span class="sxs-lookup"><span data-stu-id="ed90a-124">Add PHP web or worker roles</span></span>
<span data-ttu-id="ed90a-125">tooadd en PHP tooa webbrollsprojektet, kör följande kommando från i hello projektets rotkatalog hello:</span><span class="sxs-lookup"><span data-stu-id="ed90a-125">tooadd a PHP web role tooa project, run hello following command from within hello project's root directory:</span></span>

    PS C:\myProject> Add-AzurePHPWebRole roleName

<span data-ttu-id="ed90a-126">Använd följande kommando för en arbetsroll:</span><span class="sxs-lookup"><span data-stu-id="ed90a-126">For a worker role, use this command:</span></span>

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> <span data-ttu-id="ed90a-127">Hej `roleName` parametern är valfri.</span><span class="sxs-lookup"><span data-stu-id="ed90a-127">hello `roleName` parameter is optional.</span></span> <span data-ttu-id="ed90a-128">Om det utelämnas ska hello rollnamn skapas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ed90a-128">If it is omitted, hello role name will be automatically generated.</span></span> <span data-ttu-id="ed90a-129">hello första webbroll skapat kommer att `WebRole1`, hello andra kommer att `WebRole2`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ed90a-129">hello first web role created will be `WebRole1`, hello second will be `WebRole2`, and so on.</span></span> <span data-ttu-id="ed90a-130">hello första arbetsrollen skapat kommer att `WorkerRole1`, hello andra kommer att `WorkerRole2`och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ed90a-130">hello first worker role created will be `WorkerRole1`, hello second will be `WorkerRole2`, and so on.</span></span>
>
>

## <a name="specify-hello-built-in-php-version"></a><span data-ttu-id="ed90a-131">Ange hello inbyggda PHP-version</span><span class="sxs-lookup"><span data-stu-id="ed90a-131">Specify hello built-in PHP version</span></span>
<span data-ttu-id="ed90a-132">När du lägger till ett PHP webb eller arbetare tooa rollprojekt ändras hello projektet konfigurationsfiler så att PHP kommer att installeras på varje webb eller arbetare instans av programmet när det distribueras.</span><span class="sxs-lookup"><span data-stu-id="ed90a-132">When you add a PHP web or worker role tooa project, hello project's configuration files are modified so that PHP will be installed on each web or worker instance of your application when it is deployed.</span></span> <span data-ttu-id="ed90a-133">toosee hello versionen av PHP som installeras som standard kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ed90a-133">toosee hello version of PHP that will be installed by default, run hello following command:</span></span>

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

<span data-ttu-id="ed90a-134">hello utdata från kommandot hello ser ut ungefär toowhat visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ed90a-134">hello output from hello command above will look similar toowhat is shown below.</span></span> <span data-ttu-id="ed90a-135">I det här exemplet hello `IsDefault` flaggan anges för`true` för PHP 5.3.17, som anger att det blir hello PHP standardversionen installerad.</span><span class="sxs-lookup"><span data-stu-id="ed90a-135">In this example, hello `IsDefault` flag is set too`true` for PHP 5.3.17, indicating that it will be hello default PHP version installed.</span></span>

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

<span data-ttu-id="ed90a-136">Du kan ange hello PHP runtime version tooany av hello PHP-versionerna.</span><span class="sxs-lookup"><span data-stu-id="ed90a-136">You can set hello PHP runtime version tooany of hello PHP versions that are listed.</span></span> <span data-ttu-id="ed90a-137">Till exempel tooset hello PHP-version (för en roll med namnet hello `roleName`) too5.4.0, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ed90a-137">For example, tooset hello PHP version (for a role with hello name `roleName`) too5.4.0, use hello following command:</span></span>

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> <span data-ttu-id="ed90a-138">Tillgängliga PHP-versioner kan ändras i framtida hello.</span><span class="sxs-lookup"><span data-stu-id="ed90a-138">Available PHP versions may change in hello future.</span></span>
>
>

## <a name="customize-hello-built-in-php-runtime"></a><span data-ttu-id="ed90a-139">Anpassa hello inbyggda PHP-körning</span><span class="sxs-lookup"><span data-stu-id="ed90a-139">Customize hello built-in PHP runtime</span></span>
<span data-ttu-id="ed90a-140">Du har fullständig kontroll över hello konfigurationen av hello PHP-körning som installeras när du gör hello ovan, även ändringar av `php.ini` inställningar och aktivering av tillägg.</span><span class="sxs-lookup"><span data-stu-id="ed90a-140">You have complete control over hello configuration of hello PHP runtime that is installed when you follow hello steps above, including modification of `php.ini` settings and enabling of extensions.</span></span>

<span data-ttu-id="ed90a-141">toocustomize Hej inbyggda PHP-körning, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="ed90a-141">toocustomize hello built-in PHP runtime, follow these steps:</span></span>

1. <span data-ttu-id="ed90a-142">Lägg till en ny mapp med namnet `php`, toohello `bin` katalogen för webbrollen.</span><span class="sxs-lookup"><span data-stu-id="ed90a-142">Add a new folder, named `php`, toohello `bin` directory of your web role.</span></span> <span data-ttu-id="ed90a-143">För en arbetsroll lägga till den toohello roll rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="ed90a-143">For a worker role, add it toohello role's root directory.</span></span>
2. <span data-ttu-id="ed90a-144">I hello `php` mapp, skapa en annan mapp med namnet `ext`.</span><span class="sxs-lookup"><span data-stu-id="ed90a-144">In hello `php` folder, create another folder called `ext`.</span></span> <span data-ttu-id="ed90a-145">Placera alla `.dll` tilläggsfiler (t.ex. `php_mongo.dll`) som du vill tooenable i den här mappen.</span><span class="sxs-lookup"><span data-stu-id="ed90a-145">Put any `.dll` extension files (e.g., `php_mongo.dll`) that you want tooenable in this folder.</span></span>
3. <span data-ttu-id="ed90a-146">Lägg till en `php.ini` filen toohello `php` mapp.</span><span class="sxs-lookup"><span data-stu-id="ed90a-146">Add a `php.ini` file toohello `php` folder.</span></span> <span data-ttu-id="ed90a-147">Aktivera alla anpassade tillägg och ange en PHP-direktiv i filen.</span><span class="sxs-lookup"><span data-stu-id="ed90a-147">Enable any custom extensions and set any PHP directives in this file.</span></span> <span data-ttu-id="ed90a-148">Om du vill ha tooturn exempelvis `display_errors` på och aktivera hello `php_mongo.dll` tillägg, hello innehållet i din `php.ini` filen blir som följer:</span><span class="sxs-lookup"><span data-stu-id="ed90a-148">For example, if you wanted tooturn `display_errors` on and enable hello `php_mongo.dll` extension, hello contents of your `php.ini` file would be as follows:</span></span>

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> <span data-ttu-id="ed90a-149">Alla inställningar som du inte uttryckligen anger i hello `php.ini` fil att du anger kommer automatiskt att ange standardvärden för tootheir.</span><span class="sxs-lookup"><span data-stu-id="ed90a-149">Any settings that you don't explicitly set in hello `php.ini` file that you provide will automatically be set tootheir default values.</span></span> <span data-ttu-id="ed90a-150">Tänk dock på att du kan lägga till en fullständig `php.ini` fil.</span><span class="sxs-lookup"><span data-stu-id="ed90a-150">However, keep in mind that you can add a complete `php.ini` file.</span></span>
>
>

## <a name="use-your-own-php-runtime"></a><span data-ttu-id="ed90a-151">Använda din egen PHP-körning</span><span class="sxs-lookup"><span data-stu-id="ed90a-151">Use your own PHP runtime</span></span>
<span data-ttu-id="ed90a-152">I vissa fall kan i stället för att välja en inbyggd PHP-körning och konfigurera det som beskrivs ovan, kan du tooprovide egna PHP-körning.</span><span class="sxs-lookup"><span data-stu-id="ed90a-152">In some cases, instead of selecting a built-in PHP runtime and configuring it as described above, you may want tooprovide your own PHP runtime.</span></span> <span data-ttu-id="ed90a-153">Du kan till exempel använda hello samma PHP-körning i en webb- eller arbetarroll roll som du använder i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ed90a-153">For example, you can use hello same PHP runtime in a web or worker role that you use in your development environment.</span></span> <span data-ttu-id="ed90a-154">Detta gör det enklare tooensure att hello program inte att ändra beteende i din produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="ed90a-154">This makes it easier tooensure that hello application will not change behavior in your production environment.</span></span>

### <a name="configure-a-web-role-toouse-your-own-php-runtime"></a><span data-ttu-id="ed90a-155">Konfigurera en webbserver-rollen toouse egna PHP-körning</span><span class="sxs-lookup"><span data-stu-id="ed90a-155">Configure a web role toouse your own PHP runtime</span></span>
<span data-ttu-id="ed90a-156">tooconfigure en webbserver-rollen toouse en PHP-körning som du anger så här:</span><span class="sxs-lookup"><span data-stu-id="ed90a-156">tooconfigure a web role toouse a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="ed90a-157">Skapa ett Azure Service-projekt och Lägg till en PHP-webbroll enligt beskrivningen tidigare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ed90a-157">Create an Azure Service project and add a PHP web role as described previously in this topic.</span></span>
2. <span data-ttu-id="ed90a-158">Skapa en `php` mapp i hello `bin` mapp som finns i rotkatalogen för din webbserver-rollen och Lägg sedan till din PHP-körning (alla binärfiler, konfigurationsfiler, undermappar, etc.) toohello `php` mapp.</span><span class="sxs-lookup"><span data-stu-id="ed90a-158">Create a `php` folder in hello `bin` folder that is in your web role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) toohello `php` folder.</span></span>
3. <span data-ttu-id="ed90a-159">(VALFRITT) Om PHP-körning använder hello [Drivers Microsoft för PHP för SQL Server][sqlsrv drivers], behöver du tooconfigure din webbserver-rollen tooinstall [SQL Server Native Client 2012] [ sql native client] när den har etablerats.</span><span class="sxs-lookup"><span data-stu-id="ed90a-159">(OPTIONAL) If your PHP runtime uses hello [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need tooconfigure your web role tooinstall [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="ed90a-160">toodo detta, Lägg till hello [sqlncli.msi x64 installer] toohello `bin` mapp i rotkatalogen för din webbroll.</span><span class="sxs-lookup"><span data-stu-id="ed90a-160">toodo this, add hello [sqlncli.msi x64 installer] toohello `bin` folder in your web role's root directory.</span></span> <span data-ttu-id="ed90a-161">hello startskript som beskrivs i nästa steg i hello körs tyst hello installer när hello rollen etableras.</span><span class="sxs-lookup"><span data-stu-id="ed90a-161">hello startup script described in hello next step will silently run hello installer when hello role is provisioned.</span></span> <span data-ttu-id="ed90a-162">Om PHP-körning inte använder hello Microsoft Drivers för PHP för SQL Server, kan du ta bort hello följande rad från hello-skript som visas i nästa steg i hello:</span><span class="sxs-lookup"><span data-stu-id="ed90a-162">If your PHP runtime does not use hello Microsoft Drivers for PHP for SQL Server, you can remove hello following line from hello script shown in hello next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="ed90a-163">Definiera en startåtgärd som konfigurerar [Internet Information Services (IIS)] [ iis.net] toouse din PHP runtime toohandle begäranden om `.php` sidor.</span><span class="sxs-lookup"><span data-stu-id="ed90a-163">Define a startup task that configures [Internet Information Services (IIS)][iis.net] toouse your PHP runtime toohandle requests for `.php` pages.</span></span> <span data-ttu-id="ed90a-164">toodo detta, öppna hello `setup_web.cmd` fil (i hello `bin` filen i rotkatalogen för din webbroll) i en textredigerare och Ersätt innehållet med hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="ed90a-164">toodo this, open hello `setup_web.cmd` file (in hello `bin` file of your web role's root directory) in a text editor and replace its contents with hello following script:</span></span>

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
5. <span data-ttu-id="ed90a-165">Lägg till ditt program filer tooyour webbroll rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="ed90a-165">Add your application files tooyour web role's root directory.</span></span> <span data-ttu-id="ed90a-166">Detta kan vara hello webbserverns rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="ed90a-166">This will be hello web server's root directory.</span></span>
6. <span data-ttu-id="ed90a-167">Publicera programmet enligt beskrivningen i hello [publicera programmet](#publish-your-application) nedan.</span><span class="sxs-lookup"><span data-stu-id="ed90a-167">Publish your application as described in hello [Publish your application](#publish-your-application) section below.</span></span>

> [!NOTE]
> <span data-ttu-id="ed90a-168">Hej `download.ps1` skript (i hello `bin` mappen hello webbroll rotkatalogen) kan tas bort när du har följt hello stegen ovan för att använda din egen PHP-körning.</span><span class="sxs-lookup"><span data-stu-id="ed90a-168">hello `download.ps1` script (in hello `bin` folder of hello web role's root directory) can be deleted after you follow hello steps described above for using your own PHP runtime.</span></span>
>
>

### <a name="configure-a-worker-role-toouse-your-own-php-runtime"></a><span data-ttu-id="ed90a-169">Konfigurera en worker-rollen toouse egna PHP-körning</span><span class="sxs-lookup"><span data-stu-id="ed90a-169">Configure a worker role toouse your own PHP runtime</span></span>
<span data-ttu-id="ed90a-170">tooconfigure en worker-rollen toouse en PHP-körning som du anger så här:</span><span class="sxs-lookup"><span data-stu-id="ed90a-170">tooconfigure a worker role toouse a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="ed90a-171">Skapa ett Azure Service-projekt och Lägg till en PHP-arbetsroll som beskrivs tidigare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="ed90a-171">Create an Azure Service project and add a PHP worker role as described previously in this topic.</span></span>
2. <span data-ttu-id="ed90a-172">Skapa en `php` mapp i rotkatalogen för hello worker rollen och Lägg sedan till din PHP-körning (alla binärfiler, konfigurationsfiler, undermappar, etc.) toohello `php` mapp.</span><span class="sxs-lookup"><span data-stu-id="ed90a-172">Create a `php` folder in hello worker role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) toohello `php` folder.</span></span>
3. <span data-ttu-id="ed90a-173">(VALFRITT) Om PHP-körning använder [Drivers Microsoft för PHP för SQL Server][sqlsrv drivers], behöver du tooconfigure din worker-rollen tooinstall [SQL Server Native Client 2012] [ sql native client] när den har etablerats.</span><span class="sxs-lookup"><span data-stu-id="ed90a-173">(OPTIONAL) If your PHP runtime uses [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need tooconfigure your worker role tooinstall [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="ed90a-174">toodo detta, Lägg till hello [sqlncli.msi x64 installer] toohello worker rollen rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="ed90a-174">toodo this, add hello [sqlncli.msi x64 installer] toohello worker role's root directory.</span></span> <span data-ttu-id="ed90a-175">hello startskript som beskrivs i nästa steg i hello körs tyst hello installer när hello rollen etableras.</span><span class="sxs-lookup"><span data-stu-id="ed90a-175">hello startup script described in hello next step will silently run hello installer when hello role is provisioned.</span></span> <span data-ttu-id="ed90a-176">Om PHP-körning inte använder hello Microsoft Drivers för PHP för SQL Server, kan du ta bort hello följande rad från hello-skript som visas i nästa steg i hello:</span><span class="sxs-lookup"><span data-stu-id="ed90a-176">If your PHP runtime does not use hello Microsoft Drivers for PHP for SQL Server, you can remove hello following line from hello script shown in hello next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="ed90a-177">Definiera en startåtgärd som lägger till din `php.exe` körbara toohello worker rollen PATH-miljövariabeln när hello rollen etableras.</span><span class="sxs-lookup"><span data-stu-id="ed90a-177">Define a startup task that adds your `php.exe` executable toohello worker role's PATH environment variable when hello role is provisioned.</span></span> <span data-ttu-id="ed90a-178">toodo detta, öppna hello `setup_worker.cmd` filen (i rotkatalogen för hello worker rollen) i en textredigerare och Ersätt innehållet med hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="ed90a-178">toodo this, open hello `setup_worker.cmd` file (in hello worker role's root directory) in a text editor and replace its contents with hello following script:</span></span>

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
5. <span data-ttu-id="ed90a-179">Lägg till rotkatalogen för din ansökan filer tooyour worker roll.</span><span class="sxs-lookup"><span data-stu-id="ed90a-179">Add your application files tooyour worker role's root directory.</span></span>
6. <span data-ttu-id="ed90a-180">Publicera programmet enligt beskrivningen i hello [publicera programmet](#publish-your-application) nedan.</span><span class="sxs-lookup"><span data-stu-id="ed90a-180">Publish your application as described in hello [Publish your application](#publish-your-application) section below.</span></span>

## <a name="run-your-application-in-hello-compute-and-storage-emulators"></a><span data-ttu-id="ed90a-181">Kör ditt program i hello emulatorerna beräkning och lagring</span><span class="sxs-lookup"><span data-stu-id="ed90a-181">Run your application in hello compute and storage emulators</span></span>
<span data-ttu-id="ed90a-182">hello ange Azure emulatorerna en lokal miljö där du kan testa din Azure-program innan du distribuerar toohello moln.</span><span class="sxs-lookup"><span data-stu-id="ed90a-182">hello Azure emulators provide a local environment in which you can test your Azure application before you deploy it toohello cloud.</span></span> <span data-ttu-id="ed90a-183">Det finns några skillnader mellan hello emulatorerna och hello Azure-miljön.</span><span class="sxs-lookup"><span data-stu-id="ed90a-183">There are some differences between hello emulators and hello Azure environment.</span></span> <span data-ttu-id="ed90a-184">toounderstand detta bättre, se [Använd hello Azure storage-emulatorn för utveckling och testning](storage/common/storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="ed90a-184">toounderstand this better, see [Use hello Azure storage emulator for development and testing](storage/common/storage-use-emulator.md).</span></span>

<span data-ttu-id="ed90a-185">Observera att du måste ha PHP installerat lokalt toouse hello-beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="ed90a-185">Note that you must have PHP installed locally toouse hello compute emulator.</span></span> <span data-ttu-id="ed90a-186">Hej beräkningsemulatorn använder din lokala PHP-installation toorun ditt program.</span><span class="sxs-lookup"><span data-stu-id="ed90a-186">hello compute emulator will use your local PHP installation toorun your application.</span></span>

<span data-ttu-id="ed90a-187">toorun ditt projekt i hello emulatorerna köra följande kommando från projektets rotkatalog hello:</span><span class="sxs-lookup"><span data-stu-id="ed90a-187">toorun your project in hello emulators, execute hello following command from your project's root directory:</span></span>

    PS C:\MyProject> Start-AzureEmulator

<span data-ttu-id="ed90a-188">Utdata liknande toothis visas:</span><span class="sxs-lookup"><span data-stu-id="ed90a-188">You will see output similar toothis:</span></span>

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

<span data-ttu-id="ed90a-189">Du kan se ditt program som körs i hello-emulatorn genom att öppna en webbläsare och surfning toohello lokal adress visas i hello utdata (`http://127.0.0.1:81` i hello exempel på utdata ovan).</span><span class="sxs-lookup"><span data-stu-id="ed90a-189">You can see your application running in hello emulator by opening a web browser and browsing toohello local address shown in hello output (`http://127.0.0.1:81` in hello example output above).</span></span>

<span data-ttu-id="ed90a-190">toostop hello emulatorerna kör kommandot:</span><span class="sxs-lookup"><span data-stu-id="ed90a-190">toostop hello emulators, execute this command:</span></span>

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a><span data-ttu-id="ed90a-191">Publicera programmet</span><span class="sxs-lookup"><span data-stu-id="ed90a-191">Publish your application</span></span>
<span data-ttu-id="ed90a-192">toopublish ditt program behöver du toofirst importera dina publiceringsinställningar med hjälp av hello [importera AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ed90a-192">toopublish your application, you need toofirst import your publish settings by using hello [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet.</span></span> <span data-ttu-id="ed90a-193">Sedan kan du publicera dina program med hjälp av hello [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ed90a-193">Then you can publish your application by using hello [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet.</span></span> <span data-ttu-id="ed90a-194">Information om att logga in finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ed90a-194">For information about signing in, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed90a-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ed90a-195">Next steps</span></span>
<span data-ttu-id="ed90a-196">Mer information finns i hello [PHP Developer Center](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="ed90a-196">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

[Azure SDK för PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[service definition (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[tjänstkonfiguration (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648
