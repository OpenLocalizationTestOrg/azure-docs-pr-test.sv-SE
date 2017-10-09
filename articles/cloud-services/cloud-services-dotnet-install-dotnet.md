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
# <a name="install-net-on-azure-cloud-services-roles"></a>Installera .NET på Azure Cloud Services roller
Den här artikeln beskriver hur tooinstall versioner av .NET Framework som inte ingår i hello Azure Gästoperativsystem. Du kan använda .NET på hello Gästoperativsystem tooconfigure dina webb- och arbetsroller molntjänstroller.

Du kan till exempel installera .NET 4.6.1 på hello gäst-OS-familjen 4, som inte ingår i någon version av .NET 4.6. (hello gäst-OS-familjen 5 kommer med .NET 4.6). Hello senaste informationen om hello Azure Gästoperativsystem släpper ser hello [Azure Gästoperativsystem släpper nyheter](cloud-services-guestos-update-matrix.md). 

>[!IMPORTANT]
>hello Azure SDK 2.9 innehåller en begränsning för hur du distribuerar .NET 4.6 på hello gäst-OS-familjen 4 eller tidigare. En korrigering för hello begränsning är tillgängligt på hello [Microsoft Docs](https://github.com/MicrosoftDocs/azure-cloud-services-files/tree/master/Azure%20Targets%20SDK%202.9) plats.

tooinstall .NET på din webb- och arbetsroller roller inkluderar hello .NET webbinstallationsprogram som en del av ditt molntjänstprojekt. Starta hello installationsprogrammet som en del av hello rollen startade uppgifter. 

## <a name="add-hello-net-installer-tooyour-project"></a>Lägga till hello .NET installer tooyour projekt
toodownload hello webbinstallationsprogrammet för hello .NET Framework, Välj hello-version som du vill tooinstall:

* [.NET 4.7 webbinstallationsprogram](http://go.microsoft.com/fwlink/?LinkId=825298)
* [.NET 4.6.1 web installer](http://go.microsoft.com/fwlink/?LinkId=671729)

tooadd hello installationsprogram för ett *web* roll:
  1. I **Solution Explorer**under **roller** i ditt molntjänstprojekt högerklickar du på din *web* rollen och välj **Lägg till** > **ny mapp**. Skapa en mapp med namnet **bin**.
  2. Högerklicka på hello bin-mappen och välj **Lägg till** > **befintlig artikel**. Välj hello .NET installationsprogram och Lägg den toohello bin-mappen.
  
tooadd hello installationsprogram för ett *worker* roll:
* Högerklicka på din *worker* rollen och välj **Lägg till** > **befintlig artikel**. Välj hello .NET installationsprogram och Lägg den toohello roll. 

Om filer som läggs till i den här sätt toohello roll innehåll mappen, läggs de automatiskt till tooyour cloud service-paketet. hello-filerna är då distribuerade tooa konsekvent plats på hello virtuell dator. Upprepa proceduren för varje roll för webb- och arbetsroller i din molntjänst så att alla roller har en kopia av hello installer.

> [!NOTE]
> Du bör installera .NET 4.6.1 på rolltjänst för molnet även om ditt program mål .NET 4.6. hello Gästoperativsystem innehåller hello Knowledge Base [uppdatera 3098779](https://support.microsoft.com/kb/3098779) och [uppdatera 3097997](https://support.microsoft.com/kb/3097997). Problem kan uppstå när du kör .NET-program om .NET 4.6 installeras ovanpå hello Knowledge Base-uppdateringar. tooavoid dessa frågor, installera .NET 4.6.1 snarare än version 4.6. Mer information finns i hello [Knowledge Base-artikel 3118750](https://support.microsoft.com/kb/3118750).
> 
> 

![Rollen innehållet med installer-filer][1]

## <a name="define-startup-tasks-for-your-roles"></a>Ange start-aktiviteter för dina roller
Du kan använda Start uppgifter tooperform åtgärder innan du startar en roll. Installera hello .NET Framework som en del av hello startaktivitet garanterar att hello framework har installerats innan alla programkoden körs. Mer information om uppgifter för start finns [köra startade uppgifter i Azure](cloud-services-startup-tasks.md). 

1. Lägg till följande innehåll toohello ServiceDefinition.csdef under hello hello **WebRole** eller **WorkerRole** nod för alla roller:
   
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
   
    hello föregående konfiguration kör hello konsolen kommandot `install.cmd` med administratören privilegier tooinstall hello .NET Framework. hello konfigurationen skapar också en **LocalStorage** element med namnet **NETFXInstall**. hello startskript anger hello tillfällig mapp toouse resursen lokal lagring. 
    
    > [!IMPORTANT]
    > tooensure korrigera installation av hello framework, ange hello storleken på den här resursen tooat minst 1 024 MB.
    
    Mer information om uppgifter för start finns [vanliga Azure Cloud Services startade uppgifter](cloud-services-startup-tasks-common.md).

2. Skapa en fil med namnet **install.cmd** och Lägg till följande hello installera toohello skriptfilen.

    hello skriptet kontrollerar om hello angiven version av hello .NET Framework är installerad på datorn hello genom att fråga hello registret. Om hello .NET version inte är installerat har hello .NET webbinstallationsprogram öppnats. toohelp felsöka eventuella problem, hello skriptet loggar alla aktiviteten toohello filen startuptasklog-(aktuellt datum och tid) .txt som lagras i **InstallLogs** lokal lagring.

    > [!IMPORTANT]
    > Använd en grundläggande textredigerare som Windows toocreate hello install.cmd anteckningsfilen. Om du använder Visual Studio toocreate en textfil och ändra hello tillägget too.cmd kan hello filen fortfarande innehålla en UTF-8 byte-ordningsmarkering. Detta märke kan orsaka ett fel när hello första raden i hello skript körs. tooavoid felet, se hello första raden i hello skript en b-instruktion som kan hoppas över av hello byte-ordning bearbetning. 
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
   > Det här skriptet visar hur tooinstall .NET 4.5.2 eller version 4.6 för att undvika störningar, även om .NET 4.5.2 finns redan på hello Azure Gästoperativsystem. Direkt installerar du .NET 4.6.1 snarare än version 4.6, enligt beskrivningen i hello [Knowledge Base-artikel 3118750](https://support.microsoft.com/kb/3118750).
   > 
   > 

3. Lägg till hello install.cmd filen tooeach roll med hjälp av **Lägg till** > **befintligt objekt** i **Solution Explorer** som beskrivits tidigare i det här avsnittet. 

    När det här steget är klar kan ha alla roller hello .NET installationsfil och hello install.cmd filen.

   ![Rollen innehållet med alla filer][2]

## <a name="configure-diagnostics-tootransfer-startup-logs-tooblob-storage"></a>Konfigurera diagnostik tootransfer Start loggar tooBlob lagring
toosimplify felsökning av installationsproblem, kan du konfigurera Azure-diagnostik tootransfer alla loggfiler som genererats av hello startade skript eller hello .NET installer tooAzure Blob storage. Med den här metoden kan visa du hello loggar genom att hämta hello loggfiler från Blob storage istället tooremote skrivbordet till hello roll.

tooconfigure Diagnostics, öppna hello diagnostics.wadcfgx filen och Lägg till följande innehåll under hello hello **kataloger** nod: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Den här XML konfigurerar diagnostik tootransfer hello filer i hello loggkatalogen i hello **NETFXInstall** resurs toohello diagnostiklagringskonto i hello **netfx installera** blob-behållare.

## <a name="deploy-your-cloud-service"></a>Distribuera din molntjänst
När du distribuerar din molntjänst kan installera hello Start uppgifter hello .NET Framework om den inte redan är installerad. Dina molntjänstroller finns i hello *upptagen* tillstånd medan hello framework installeras. Om hello framework-installation kräver en omstart, hello service roller kan också starta om. 

## <a name="additional-resources"></a>Ytterligare resurser
* [Installera hello .NET Framework][Installing hello .NET Framework]
* [Ta reda på vilka versioner av .NET Framework är installerad][How to: Determine Which .NET Framework Versions Are Installed]
* [Felsökning av .NET Framework-installationer][Troubleshooting .NET Framework Installations]

[How to: Determine Which .NET Framework Versions Are Installed]: https://msdn.microsoft.com/library/hh925568.aspx
[Installing hello .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Troubleshooting .NET Framework Installations]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png
