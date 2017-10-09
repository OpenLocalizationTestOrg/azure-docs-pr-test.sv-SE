---
title: "aaaInstall .NET på Azure Cloud Services roller | Microsoft Docs"
description: "Den här artikeln beskrivs hur toomanually installera hello .NET Framework på dina webb- och arbetsroller molntjänstroller"
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: 
ms.assetid: 8d1243dc-879c-4d1f-9ed0-eecd1f6a6653
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: adegeo
ms.openlocfilehash: 45f0f30221292f98c591511b091b02ebe1c1272c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-net-on-azure-cloud-services-roles"></a><span data-ttu-id="3f15e-103">Installera .NET på Azure Cloud Services roller</span><span class="sxs-lookup"><span data-stu-id="3f15e-103">Install .NET on Azure Cloud Services roles</span></span>
<span data-ttu-id="3f15e-104">Den här artikeln beskriver hur tooinstall versioner av .NET Framework som inte ingår i hello Azure Gästoperativsystem.</span><span class="sxs-lookup"><span data-stu-id="3f15e-104">This article describes how tooinstall versions of .NET Framework that don't come with hello Azure Guest OS.</span></span> <span data-ttu-id="3f15e-105">Du kan använda .NET på hello Gästoperativsystem tooconfigure dina webb- och arbetsroller molntjänstroller.</span><span class="sxs-lookup"><span data-stu-id="3f15e-105">You can use .NET on hello Guest OS tooconfigure your cloud service web and worker roles.</span></span>

<span data-ttu-id="3f15e-106">Du kan till exempel installera .NET 4.6.1 på hello gäst-OS-familjen 4, som inte ingår i någon version av .NET 4.6.</span><span class="sxs-lookup"><span data-stu-id="3f15e-106">For example, you can install .NET 4.6.1 on hello Guest OS family 4, which doesn't come with any release of .NET 4.6.</span></span> <span data-ttu-id="3f15e-107">(hello gäst-OS-familjen 5 kommer med .NET 4.6). Hello senaste informationen om hello Azure Gästoperativsystem släpper ser hello [Azure Gästoperativsystem släpper nyheter](cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="3f15e-107">(hello Guest OS family 5 does come with .NET 4.6.) For hello latest information on hello Azure Guest OS releases, see hello [Azure Guest OS release news](cloud-services-guestos-update-matrix.md).</span></span> 

>[!IMPORTANT]
><span data-ttu-id="3f15e-108">hello Azure SDK 2.9 innehåller en begränsning för hur du distribuerar .NET 4.6 på hello gäst-OS-familjen 4 eller tidigare.</span><span class="sxs-lookup"><span data-stu-id="3f15e-108">hello Azure SDK 2.9 contains a restriction on deploying .NET 4.6 on hello Guest OS family 4 or earlier.</span></span> <span data-ttu-id="3f15e-109">En korrigering för hello begränsning är tillgängligt på hello [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) plats.</span><span class="sxs-lookup"><span data-stu-id="3f15e-109">A fix for hello restriction is available on hello [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) site.</span></span>

<span data-ttu-id="3f15e-110">tooinstall .NET på din webb- och arbetsroller roller inkluderar hello .NET webbinstallationsprogram som en del av ditt molntjänstprojekt.</span><span class="sxs-lookup"><span data-stu-id="3f15e-110">tooinstall .NET on your web and worker roles, include hello .NET web installer as part of your cloud service project.</span></span> <span data-ttu-id="3f15e-111">Starta hello installationsprogrammet som en del av hello rollen startade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="3f15e-111">Start hello installer as part of hello role's startup tasks.</span></span> 

## <a name="add-hello-net-installer-tooyour-project"></a><span data-ttu-id="3f15e-112">Lägga till hello .NET installer tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="3f15e-112">Add hello .NET installer tooyour project</span></span>
<span data-ttu-id="3f15e-113">toodownload hello webbinstallationsprogrammet för hello .NET Framework, Välj hello-version som du vill tooinstall:</span><span class="sxs-lookup"><span data-stu-id="3f15e-113">toodownload hello web installer for hello .NET Framework, choose hello version that you want tooinstall:</span></span>

* [<span data-ttu-id="3f15e-114">.NET 4.7 webbinstallationsprogram</span><span class="sxs-lookup"><span data-stu-id="3f15e-114">.NET 4.7 web installer</span></span>](http://go.microsoft.com/fwlink/?LinkId=825298)
* [<span data-ttu-id="3f15e-115">.NET 4.6.1 web installer</span><span class="sxs-lookup"><span data-stu-id="3f15e-115">.NET 4.6.1 web installer</span></span>](http://go.microsoft.com/fwlink/?LinkId=671729)

<span data-ttu-id="3f15e-116">tooadd hello installationsprogram för ett *web* roll:</span><span class="sxs-lookup"><span data-stu-id="3f15e-116">tooadd hello installer for a *web* role:</span></span>
  1. <span data-ttu-id="3f15e-117">I **Solution Explorer**under **roller** i ditt molntjänstprojekt högerklickar du på din *web* rollen och välj **Lägg till** > **ny mapp**.</span><span class="sxs-lookup"><span data-stu-id="3f15e-117">In **Solution Explorer**, under **Roles** in your cloud service project, right-click your *web* role and select **Add** > **New Folder**.</span></span> <span data-ttu-id="3f15e-118">Skapa en mapp med namnet **bin**.</span><span class="sxs-lookup"><span data-stu-id="3f15e-118">Create a folder named **bin**.</span></span>
  2. <span data-ttu-id="3f15e-119">Högerklicka på hello bin-mappen och välj **Lägg till** > **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="3f15e-119">Right-click hello bin folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="3f15e-120">Välj hello .NET installationsprogram och Lägg den toohello bin-mappen.</span><span class="sxs-lookup"><span data-stu-id="3f15e-120">Select hello .NET installer and add it toohello bin folder.</span></span>
  
<span data-ttu-id="3f15e-121">tooadd hello installationsprogram för ett *worker* roll:</span><span class="sxs-lookup"><span data-stu-id="3f15e-121">tooadd hello installer for a *worker* role:</span></span>
* <span data-ttu-id="3f15e-122">Högerklicka på din *worker* rollen och välj **Lägg till** > **befintlig artikel**.</span><span class="sxs-lookup"><span data-stu-id="3f15e-122">Right-click your *worker* role and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="3f15e-123">Välj hello .NET installationsprogram och Lägg den toohello roll.</span><span class="sxs-lookup"><span data-stu-id="3f15e-123">Select hello .NET installer and add it toohello role.</span></span> 

<span data-ttu-id="3f15e-124">Om filer som läggs till i den här sätt toohello roll innehåll mappen, läggs de automatiskt till tooyour cloud service-paketet.</span><span class="sxs-lookup"><span data-stu-id="3f15e-124">When files are added in this way toohello role content folder, they're automatically added tooyour cloud service package.</span></span> <span data-ttu-id="3f15e-125">hello-filerna är då distribuerade tooa konsekvent plats på hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3f15e-125">hello files are then deployed tooa consistent location on hello virtual machine.</span></span> <span data-ttu-id="3f15e-126">Upprepa proceduren för varje roll för webb- och arbetsroller i din molntjänst så att alla roller har en kopia av hello installer.</span><span class="sxs-lookup"><span data-stu-id="3f15e-126">Repeat this process for each web and worker role in your cloud service so that all roles have a copy of hello installer.</span></span>

> [!NOTE]
> <span data-ttu-id="3f15e-127">Du bör installera .NET 4.6.1 på rolltjänst för molnet även om ditt program mål .NET 4.6.</span><span class="sxs-lookup"><span data-stu-id="3f15e-127">You should install .NET 4.6.1 on your cloud service role even if your application targets .NET 4.6.</span></span> <span data-ttu-id="3f15e-128">hello Gästoperativsystem innehåller hello Knowledge Base [uppdatera 3098779](https://support.microsoft.com/kb/3098779) och [uppdatera 3097997](https://support.microsoft.com/kb/3097997).</span><span class="sxs-lookup"><span data-stu-id="3f15e-128">hello Guest OS includes hello Knowledge Base [update 3098779](https://support.microsoft.com/kb/3098779) and [update 3097997](https://support.microsoft.com/kb/3097997).</span></span> <span data-ttu-id="3f15e-129">Problem kan uppstå när du kör .NET-program om .NET 4.6 installeras ovanpå hello Knowledge Base-uppdateringar.</span><span class="sxs-lookup"><span data-stu-id="3f15e-129">Issues can occur when you run your .NET applications if .NET 4.6 is installed on top of hello Knowledge Base updates.</span></span> <span data-ttu-id="3f15e-130">tooavoid dessa frågor, installera .NET 4.6.1 snarare än version 4.6.</span><span class="sxs-lookup"><span data-stu-id="3f15e-130">tooavoid these issues, install .NET 4.6.1 rather than version 4.6.</span></span> <span data-ttu-id="3f15e-131">Mer information finns i hello [Knowledge Base-artikel 3118750](https://support.microsoft.com/kb/3118750).</span><span class="sxs-lookup"><span data-stu-id="3f15e-131">For more information, see hello [Knowledge Base article 3118750](https://support.microsoft.com/kb/3118750).</span></span>
> 
> 

![Rollen innehållet med installer-filer][1]

## <a name="define-startup-tasks-for-your-roles"></a><span data-ttu-id="3f15e-133">Ange start-aktiviteter för dina roller</span><span class="sxs-lookup"><span data-stu-id="3f15e-133">Define startup tasks for your roles</span></span>
<span data-ttu-id="3f15e-134">Du kan använda Start uppgifter tooperform åtgärder innan du startar en roll.</span><span class="sxs-lookup"><span data-stu-id="3f15e-134">You can use startup tasks tooperform operations before a role starts.</span></span> <span data-ttu-id="3f15e-135">Installera hello .NET Framework som en del av hello startaktivitet garanterar att hello framework har installerats innan alla programkoden körs.</span><span class="sxs-lookup"><span data-stu-id="3f15e-135">Installing hello .NET Framework as part of hello startup task ensures that hello framework is installed before any application code is run.</span></span> <span data-ttu-id="3f15e-136">Mer information om uppgifter för start finns [köra startade uppgifter i Azure](cloud-services-startup-tasks.md).</span><span class="sxs-lookup"><span data-stu-id="3f15e-136">For more information on startup tasks, see [Run startup tasks in Azure](cloud-services-startup-tasks.md).</span></span> 

1. <span data-ttu-id="3f15e-137">Lägg till följande innehåll toohello ServiceDefinition.csdef under hello hello **WebRole** eller **WorkerRole** nod för alla roller:</span><span class="sxs-lookup"><span data-stu-id="3f15e-137">Add hello following content toohello ServiceDefinition.csdef file under hello **WebRole** or **WorkerRole** node for all roles:</span></span>
   
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```
   
    <span data-ttu-id="3f15e-138">hello föregående konfiguration kör hello konsolen kommandot `install.cmd` med administratören privilegier tooinstall hello .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3f15e-138">hello preceding configuration runs hello console command `install.cmd` with administrator privileges tooinstall hello .NET Framework.</span></span> <span data-ttu-id="3f15e-139">hello konfigurationen skapar också en **LocalStorage** element med namnet **NETFXInstall**.</span><span class="sxs-lookup"><span data-stu-id="3f15e-139">hello configuration also creates a **LocalStorage** element named **NETFXInstall**.</span></span> <span data-ttu-id="3f15e-140">hello startskript anger hello tillfällig mapp toouse resursen lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="3f15e-140">hello startup script sets hello temp folder toouse this local storage resource.</span></span> 
    
    > [!IMPORTANT]
    > <span data-ttu-id="3f15e-141">tooensure korrigera installation av hello framework, ange hello storleken på den här resursen tooat minst 1 024 MB.</span><span class="sxs-lookup"><span data-stu-id="3f15e-141">tooensure correct installation of hello framework, set hello size of this resource tooat least 1,024 MB.</span></span>
    
    <span data-ttu-id="3f15e-142">Mer information om uppgifter för start finns [vanliga Azure Cloud Services startade uppgifter](cloud-services-startup-tasks-common.md).</span><span class="sxs-lookup"><span data-stu-id="3f15e-142">For more information about startup tasks, see [Common Azure Cloud Services startup tasks](cloud-services-startup-tasks-common.md).</span></span>

2. <span data-ttu-id="3f15e-143">Skapa en fil med namnet **install.cmd** och Lägg till följande hello installera toohello skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="3f15e-143">Create a file named **install.cmd** and add hello following install script toohello file.</span></span>

    <span data-ttu-id="3f15e-144">hello skriptet kontrollerar om hello angiven version av hello .NET Framework är installerad på datorn hello genom att fråga hello registret.</span><span class="sxs-lookup"><span data-stu-id="3f15e-144">hello script checks whether hello specified version of hello .NET Framework is already installed on hello machine by querying hello registry.</span></span> <span data-ttu-id="3f15e-145">Om hello .NET version inte är installerat har hello .NET webbinstallationsprogram öppnats.</span><span class="sxs-lookup"><span data-stu-id="3f15e-145">If hello .NET version is not installed, then hello .NET web installer is opened.</span></span> <span data-ttu-id="3f15e-146">toohelp felsöka eventuella problem, hello skriptet loggar alla aktiviteten toohello filen startuptasklog-(aktuellt datum och tid) .txt som lagras i **InstallLogs** lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="3f15e-146">toohelp troubleshoot any issues, hello script logs all activity toohello file startuptasklog-(current date and time).txt that is stored in **InstallLogs** local storage.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3f15e-147">Använd en grundläggande textredigerare som Windows toocreate hello install.cmd anteckningsfilen.</span><span class="sxs-lookup"><span data-stu-id="3f15e-147">Use a basic text editor like Windows Notepad toocreate hello install.cmd file.</span></span> <span data-ttu-id="3f15e-148">Om du använder Visual Studio toocreate en textfil och ändra hello tillägget too.cmd kan hello filen fortfarande innehålla en UTF-8 byte-ordningsmarkering.</span><span class="sxs-lookup"><span data-stu-id="3f15e-148">If you use Visual Studio toocreate a text file and change hello extension too.cmd, hello file might still contain a UTF-8 byte order mark.</span></span> <span data-ttu-id="3f15e-149">Detta märke kan orsaka ett fel när hello första raden i hello skript körs.</span><span class="sxs-lookup"><span data-stu-id="3f15e-149">This mark can cause an error when hello first line of hello script is run.</span></span> <span data-ttu-id="3f15e-150">tooavoid felet, se hello första raden i hello skript en b-instruktion som kan hoppas över av hello byte-ordning bearbetning.</span><span class="sxs-lookup"><span data-stu-id="3f15e-150">tooavoid this error, make hello first line of hello script a REM statement that can be skipped by hello byte order processing.</span></span> 
    > 
    >
   
    ```cmd
    REM Set hello value of netfx tooinstall appropriate .NET Framework. 
    REM ***** tooinstall .NET 4.5.2 set hello variable netfx too"NDP452" *****
    REM ***** tooinstall .NET 4.6 set hello variable netfx too"NDP46" *****
    REM ***** tooinstall .NET 4.6.1 set hello variable netfx too"NDP461" *****
    REM ***** tooinstall .NET 4.6.2 set hello variable netfx too"NDP462" *****
    REM ***** tooinstall .NET 4.7 set hello variable netfx too"NDP47" *****
    set netfx="NDP47"

    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."

    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit

    REM ***** Needed toocorrectly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%

    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP47" goto NDP47
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp

    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp

    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp

    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"

    :NDP47
    set "netfxinstallfile=NDP47-KB3186500-Web.exe"
    set netfxregkey="0x707FE"

    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%

    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed

    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%    
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%

    :restart
    echo Restarting toocomplete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%

    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%

    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%

    :exit
    EXIT /B 0
    ```
   
   > [!NOTE]
   > <span data-ttu-id="3f15e-151">Det här skriptet visar hur tooinstall .NET 4.5.2 eller version 4.6 för att undvika störningar, även om .NET 4.5.2 finns redan på hello Azure Gästoperativsystem.</span><span class="sxs-lookup"><span data-stu-id="3f15e-151">This script shows how tooinstall .NET 4.5.2 or version 4.6 for continuity, even though .NET 4.5.2 is already available on hello Azure Guest OS.</span></span> <span data-ttu-id="3f15e-152">Direkt installerar du .NET 4.6.1 snarare än version 4.6, enligt beskrivningen i hello [Knowledge Base-artikel 3118750](https://support.microsoft.com/kb/3118750).</span><span class="sxs-lookup"><span data-stu-id="3f15e-152">You should directly install .NET 4.6.1 rather than version 4.6, as described in hello [Knowledge Base article 3118750](https://support.microsoft.com/kb/3118750).</span></span>
   > 
   > 

3. <span data-ttu-id="3f15e-153">Lägg till hello install.cmd filen tooeach roll med hjälp av **Lägg till** > **befintligt objekt** i **Solution Explorer** som beskrivits tidigare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3f15e-153">Add hello install.cmd file tooeach role by using **Add** > **Existing Item** in **Solution Explorer** as described earlier in this topic.</span></span> 

    <span data-ttu-id="3f15e-154">När det här steget är klar kan ha alla roller hello .NET installationsfil och hello install.cmd filen.</span><span class="sxs-lookup"><span data-stu-id="3f15e-154">After this step is complete, all roles should have hello .NET installer file and hello install.cmd file.</span></span>

   ![Rollen innehållet med alla filer][2]

## <a name="configure-diagnostics-tootransfer-startup-logs-tooblob-storage"></a><span data-ttu-id="3f15e-156">Konfigurera diagnostik tootransfer Start loggar tooBlob lagring</span><span class="sxs-lookup"><span data-stu-id="3f15e-156">Configure Diagnostics tootransfer startup logs tooBlob storage</span></span>
<span data-ttu-id="3f15e-157">toosimplify felsökning av installationsproblem, kan du konfigurera Azure-diagnostik tootransfer alla loggfiler som genererats av hello startade skript eller hello .NET installer tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3f15e-157">toosimplify troubleshooting installation issues, you can configure Azure Diagnostics tootransfer any log files generated by hello startup script or hello .NET installer tooAzure Blob storage.</span></span> <span data-ttu-id="3f15e-158">Med den här metoden kan visa du hello loggar genom att hämta hello loggfiler från Blob storage istället tooremote skrivbordet till hello roll.</span><span class="sxs-lookup"><span data-stu-id="3f15e-158">By using this approach, you can view hello logs by downloading hello log files from Blob storage rather than having tooremote desktop into hello role.</span></span>

<span data-ttu-id="3f15e-159">tooconfigure Diagnostics, öppna hello diagnostics.wadcfgx filen och Lägg till följande innehåll under hello hello **kataloger** nod:</span><span class="sxs-lookup"><span data-stu-id="3f15e-159">tooconfigure Diagnostics, open hello diagnostics.wadcfgx file and add hello following content under hello **Directories** node:</span></span> 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

<span data-ttu-id="3f15e-160">Den här XML konfigurerar diagnostik tootransfer hello filer i hello loggkatalogen i hello **NETFXInstall** resurs toohello diagnostiklagringskonto i hello **netfx installera** blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="3f15e-160">This XML configures Diagnostics tootransfer hello files in hello log directory in hello **NETFXInstall** resource toohello Diagnostics storage account in hello **netfx-install** blob container.</span></span>

## <a name="deploy-your-cloud-service"></a><span data-ttu-id="3f15e-161">Distribuera din molntjänst</span><span class="sxs-lookup"><span data-stu-id="3f15e-161">Deploy your cloud service</span></span>
<span data-ttu-id="3f15e-162">När du distribuerar din molntjänst kan installera hello Start uppgifter hello .NET Framework om den inte redan är installerad.</span><span class="sxs-lookup"><span data-stu-id="3f15e-162">When you deploy your cloud service, hello startup tasks install hello .NET Framework if it's not already installed.</span></span> <span data-ttu-id="3f15e-163">Dina molntjänstroller finns i hello *upptagen* tillstånd medan hello framework installeras.</span><span class="sxs-lookup"><span data-stu-id="3f15e-163">Your cloud service roles are in hello *busy* state while hello framework is being installed.</span></span> <span data-ttu-id="3f15e-164">Om hello framework-installation kräver en omstart, hello service roller kan också starta om.</span><span class="sxs-lookup"><span data-stu-id="3f15e-164">If hello framework installation requires a restart, hello service roles might also restart.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3f15e-165">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3f15e-165">Additional resources</span></span>
* <span data-ttu-id="3f15e-166">[Installera hello .NET Framework][Installing hello .NET Framework]</span><span class="sxs-lookup"><span data-stu-id="3f15e-166">[Installing hello .NET Framework][Installing hello .NET Framework]</span></span>
* <span data-ttu-id="3f15e-167">[Ta reda på vilka versioner av .NET Framework är installerad][How to: Determine Which .NET Framework Versions Are Installed]</span><span class="sxs-lookup"><span data-stu-id="3f15e-167">[Determine which .NET Framework versions are installed][How to: Determine Which .NET Framework Versions Are Installed]</span></span>
* <span data-ttu-id="3f15e-168">[Felsökning av .NET Framework-installationer][Troubleshooting .NET Framework Installations]</span><span class="sxs-lookup"><span data-stu-id="3f15e-168">[Troubleshooting .NET Framework installations][Troubleshooting .NET Framework Installations]</span></span>

[How to: Determine Which .NET Framework Versions Are Installed]: https://msdn.microsoft.com/library/hh925568.aspx
[Installing hello .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Troubleshooting .NET Framework Installations]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
