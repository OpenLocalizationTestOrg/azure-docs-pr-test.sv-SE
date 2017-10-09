---
title: "aaaCommon Start uppgifter för molntjänster | Microsoft Docs"
description: "Ger exempel för vanliga uppgifter för start du tooperform i cloud services webbroll eller worker-rollen."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: a7095dad-1ee7-4141-bc6a-ef19a6e570f1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: c80fac4079439410dfc3795e4bce0fbc07dbbfab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="common-cloud-service-startup-tasks"></a>Vanliga uppgifter för start av Molntjänsten
Den här artikeln innehåller några exempel för vanliga uppgifter för start du tooperform i Molntjänsten. Du kan använda Start uppgifter tooperform åtgärder innan du startar en roll. Åtgärder som du kanske vill tooperform inkluderar installerar en komponent, registrerar COM-komponenter, ange registernycklar eller starta en tidskrävande process. 

Se [i den här artikeln](cloud-services-startup-tasks.md) toounderstand hur start uppgifter fungerar och hur specifikt toocreate hello poster som definierar en startaktivitet.

> [!NOTE]
> Start uppgifter är inte tillämplig tooVirtual datorer, endast tooCloud Service Web- och arbetsroller.
> 

## <a name="define-environment-variables-before-a-role-starts"></a>Definiera miljövariabler innan du startar en roll
Om du behöver miljövariabler som definieras för en viss aktivitet kan använda hello [miljö] -element inuti hello [aktivitet] element.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
                <Environment>
                    <Variable name="MyEnvironmentVariable" value="MyVariableValue" />
                </Environment>
            </Task>
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Variabler kan också använda en [giltig Azure XPath-värdet](cloud-services-role-config-xpath.md) tooreference något om hello-distribution. Istället för att använda hello `value` attribut, definiera en [RoleInstanceValue] underordnat element.

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a>Konfigurera IIS-start med AppCmd.exe
Hej [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) kommandoradsverktyget kan vara används toomanage IIS-inställningarna vid start på Azure. *AppCmd.exe* ger praktiska, kommandoradsverktyg tooconfiguration inställningar för användning i startade uppgifter på Azure. Med hjälp av *AppCmd.exe*, Webbplatsinställningar kan läggas till, ändras eller tas bort för program och platser.

Det finns dock några saker toowatch ut för hello används av *AppCmd.exe* som en startåtgärd för:

* Start-uppgifter kan köras mer än en gång mellan olika omstarter. Till exempel när en roll återanvänds.
* Om en *AppCmd.exe* åtgärd utförs mer än en gång, kan ett fel genereras. Till exempel försöker tooadd ett avsnitt för*Web.config* två gånger kan genererar ett fel.
* Start misslyckas om de returnerar slutkoden noll eller **errorlevel**. Till exempel när *AppCmd.exe* genererar ett fel.

Det är en bra idé toocheck hello **errorlevel** efter att *AppCmd.exe*, som är lätt toodo om du omsluter hello-anrop för*AppCmd.exe* med en *cmd.*  fil. Om du identifierar en känd **errorlevel** svar, du kan ignorera det eller skicka tillbaka.

Hej errorlevel som returneras av *AppCmd.exe* listas i hello winerror.h fil och kan också visas på [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).

### <a name="example-of-managing-hello-error-level"></a>Exempel för att hantera hello Felnivån
Det här exemplet lägger till ett avsnitt med komprimering och en komprimering post för JSON-toohello *Web.config* fil med felhantering och loggning.

Hej relevanta avsnitt i hello [ServiceDefinition.csdef] filen visas de här, som innehåller inställningen hello [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) attribut för`elevated` toogive *AppCmd.exe*  tillräckligt toochange hello behörighetsinställningarna för hello *Web.config* fil:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="Startup.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

Hej *Startup.cmd* batch-filen använder *AppCmd.exe* tooadd ett avsnitt för komprimering och en komprimering post för JSON-toohello *Web.config* fil. hello förväntat **errorlevel** av 183 anges toozero med hello kontrollera. Exe för kommandoraden. Oväntat errorlevels är loggade tooStartupErrorLog.txt.

```cmd
REM   *** Add a compression section toohello Web.config file. ***
%windir%\system32\inetsrv\appcmd set config /section:urlCompression /doDynamicCompression:True /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1

REM   ERRORLEVEL 183 occurs when trying tooadd a section that already exists. This error is expected if this
REM   batch file were executed twice. This can occur and must be accounted for in a Azure startup
REM   task. toohandle this situation, set hello ERRORLEVEL toozero by using hello Verify command. hello Verify
REM   command will safely set hello ERRORLEVEL toozero.
IF %ERRORLEVEL% EQU 183 DO VERIFY > NUL

REM   If hello ERRORLEVEL is not zero at this point, some other error occurred.
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding a compression section toohello Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Add compression for json. ***
%windir%\system32\inetsrv\appcmd set config  -section:system.webServer/httpCompression /+"dynamicTypes.[mimeType='application/json; charset=utf-8',enabled='True']" /commit:apphost >> "%TEMP%\StartupLog.txt" 2>&1
IF %ERRORLEVEL% EQU 183 VERIFY > NUL
IF %ERRORLEVEL% NEQ 0 (
    ECHO Error adding hello JSON compression type toohello Web.config file. >> "%TEMP%\StartupLog.txt" 2>&1
    GOTO ErrorExit
)

REM   *** Exit batch file. ***
EXIT /b 0

REM   *** Log error and exit ***
:ErrorExit
REM   Report hello date, time, and ERRORLEVEL of hello error.
DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
ECHO An error occurred during startup. ERRORLEVEL = %ERRORLEVEL% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT %ERRORLEVEL%
```

## <a name="add-firewall-rules"></a>Lägga till brandväggsregler
I Azure finns effektivt två brandväggar. hello första brandväggen styr anslutningar mellan hello virtuell dator och hello utanför världen. Den här brandväggen styrs av hello [slutpunkter] element i hello [ServiceDefinition.csdef] fil.

andra hello-brandväggen styr anslutningar mellan hello virtuell dator och hello processer i den virtuella datorn. Den här brandväggen kan styras efter hello `netsh advfirewall firewall` kommandoradsverktyget.

Azure skapar brandväggsregler för hello processer som startas inom dina roller. Till exempel när du startar en tjänst eller ett program, skapas Azure automatiskt hello nödvändiga brandväggen regler tooallow som tjänsten toocommunicate med hello Internet. Om du skapar en tjänst som startas av en process utanför din roll (till exempel en COM +-tjänst eller en schemalagd aktivitet i Windows), måste du dock toomanually skapa en regel tooallow åtkomst toothat brandväggen. Brandväggsreglerna kan skapas med hjälp av en startaktivitet.

En startåtgärd som skapar en brandväggsregel måste ha en [executionContext][aktivitet] av **utökade**. Lägg till följande startade uppgiften toohello hello [ServiceDefinition.csdef] fil.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WorkerRole name="WorkerRole1">
        ...
        <Startup>
            <Task commandLine="AddFirewallRules.cmd" executionContext="elevated" taskType="simple" />
        </Startup>
    </WorkerRole>
</ServiceDefinition>
```

tooadd hello brandväggsregeln, måste du använda hello lämpliga `netsh advfirewall firewall` kommandon i kommandofilen startades. I det här exemplet kräver hello startaktivitet säkerhet och kryptering för TCP-port 80.

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a>Blockera en specifik IP-adress
Du kan begränsa en Azure web roll åtkomst tooa uppsättning angivna IP-adresser genom att ändra din IIS **web.config** fil. Du måste också toouse kommandofilen som låser upp hello **ipSecurity** avsnitt i hello **ApplicationHost.config** fil.

toodo låsa upp hello **ipSecurity** avsnitt i hello **ApplicationHost.config** fil, skapa en kommandofil som körs vid start av rollen. Skapa en mapp på rotnivå för hello web rollen kallas **Start** och skapa en batchfil som anropas i den här mappen **startup.cmd**. Lägg till den här filen tooyour Visual Studio-projekt och ange hello egenskaper för**Kopiera alltid** tooensure den ingår i paketet.

Lägg till följande startade uppgiften toohello hello [ServiceDefinition.csdef] fil.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
    <WebRole name="WebRole1">
        ...
        <Startup>
            <Task commandLine="startup.cmd" executionContext="elevated" />
        </Startup>
    </WebRole>
</ServiceDefinition>
```

Lägg till det här kommandot toohello **startup.cmd** fil:

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

Uppgiften gör hello **startup.cmd** batch-filen toobe körs varje gång hello webbroll har initierats säkerställer att hello krävs **ipSecurity** avsnitt är olåst.

Slutligen ändrar hello [system.webServer avsnittet](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) din webbroll **web.config** filen tooadd en lista över IP-adresser som beviljas åtkomst till, enligt följande exempel hello:

Det här exemplet config **kan** alla IP-adresser tooaccess hello server förutom hello två definierats

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are granted access-->
    <ipSecurity>
        <!--hello following IP addresses are denied access-->
        <add allowed="false" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="false" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

Det här exemplet config **nekar** alla IP-adresser från att komma åt hello server förutom hello två definierats.

```xml
<system.webServer>
    <security>
    <!--Unlisted IP addresses are denied access-->
    <ipSecurity allowUnlisted="false">
        <!--hello following IP addresses are granted access-->
        <add allowed="true" ipAddress="192.168.100.1" subnetMask="255.255.0.0" />
        <add allowed="true" ipAddress="192.168.100.2" subnetMask="255.255.0.0" />
    </ipSecurity>
    </security>
</system.webServer>
```

## <a name="create-a-powershell-startup-task"></a>Skapa en startåtgärd för PowerShell
Windows PowerShell-skript kan inte anropas direkt från hello [ServiceDefinition.csdef] -fil, men de kan anropas från inom en Startkommandofil.

Osignerade skript körs inte i PowerShell (som standard). Om du inte loggar skriptet, behöver du tooconfigure PowerShell toorun osignerade skript. toorun osignerade skript, hello **ExecutionPolicy** måste anges för**obegränsat**. Hej **ExecutionPolicy** inställning som du Använd baseras på hello version av Windows PowerShell.

```cmd
REM   Run an unsigned PowerShell script and log hello output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

Om du använder ett Gästoperativsystem som kör PowerShell 2.0 eller 1.0 du kan tvinga version 2 toorun, och om det är inte tillgänglig, kan du använda version 1.

```cmd
REM   Attempt tooset hello execution policy by using PowerShell version 2.0 syntax.
PowerShell -Version 2.0 -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If PowerShell version 2.0 isn't available. Set hello execution policy by using hello PowerShell
IF %ERRORLEVEL% EQU -393216 (
   PowerShell -Command "Set-ExecutionPolicy Unrestricted" >> "%TEMP%\StartupLog.txt" 2>&1
   PowerShell .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1
)

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

## <a name="create-files-in-local-storage-from-a-startup-task"></a>Skapa filer i lokal lagring från en startåtgärd
Du kan använda en lokal lagring resurs toostore filer som skapas av din startaktivitet som senare används av ditt program.

toocreate hello lokal lagringsresurs, lägga till en [LocalResources] avsnittet toohello [ServiceDefinition.csdef] filen och Lägg sedan till hello [LocalStorage] underordnat element. Ge hello lokal lagringsresurs ett unikt namn och en lämplig storlek för startaktivitet.

toouse en resurs för lokal lagring i ditt startaktivitet måste toocreate resursplats en miljö variabeln tooreference hello lokal lagring. Hej startaktivitet och hello program kan tooread och skriva filer toohello lokal lagringsresurs.

Hej relevanta avsnitt i hello **ServiceDefinition.csdef** filen visas här:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">
    ...

    <LocalResources>
      <LocalStorage name="StartupLocalStorage" sizeInMB="5"/>
    </LocalResources>

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="PathToStartupStorage">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
  </WorkerRole>
</ServiceDefinition>
```

Exempelvis detta **Startup.cmd** kommandofilen använder hello **PathToStartupStorage** miljö variabeln toocreate hello filen **MyTest.txt** på hello lokal lagring plats.

```cmd
REM   Create a simple text file.

ECHO This text will go into hello MyTest.txt file which will be in hello    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed tooby hello PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO hello contents of hello PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit hello batch file with ERRORLEVEL 0.

EXIT /b 0
```

Du kan komma åt mappen för lokal lagring från hello Azure SDK med hjälp av hello [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metod.

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-hello-emulator-or-cloud"></a>Kör i hello emulatorn eller molnet
Du kan ha din startaktivitet som utför olika steg när den körs i hello molnet jämfört med toowhen i hello beräkningsemulatorn. Exempelvis kanske toouse en ny kopia av SQL-data endast när den körs i hello-emulatorn. Eller vill du kanske toodo prestandaoptimeringar för hello moln som du inte behöver toodo vid körning i hello-emulatorn.

Den här möjligheten tooperform olika åtgärder på hello compute emulator och hello molnet kan åstadkommas genom att skapa en miljövariabel i hello [ServiceDefinition.csdef] fil. Testa att miljövariabeln för ett värde i din startaktivitet.

toocreate hello miljövariabeln, lägga till hello [variabeln]/[RoleInstanceValue] element och skapa en XPath-värdet för `/RoleEnvironment/Deployment/@emulated`. Hej värdet för hello **ComputeEmulatorRunning %** miljövariabel är `true` vid körning på hello beräkningsemulatorn och `false` när du kör på hello moln.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WorkerRole name="WorkerRole1">

    ...

    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>

  </WorkerRole>
</ServiceDefinition>
```

hello aktivitet kan nu kontrollera hello **ComputeEmulatorRunning %** miljö variabeln tooperform olika åtgärder baserat på om hello rollen körs i hello molnet eller hello emulatorn. Här är ett cmd shell-skript som söker efter att miljövariabeln.

```cmd
REM   Check if this task is running on hello compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on hello compute emulator. Perform tasks that must be run only in hello compute emulator.
) ELSE (
    REM   This task is running on hello cloud. Perform tasks that must be run only in hello cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a>Identifiera uppgiften har körts
hello-rollen kan återvinna utan en omstart orsakar din start uppgifter toorun igen. Det finns inga flaggan tooindicate som en aktivitet har redan körs på hello som värd för virtuell dator. Du kan ha vissa uppgifter där det spelar ingen roll de köras flera gånger. Men kan du köra i en situation där du behöver tooprevent en aktivitet körs mer än en gång.

hello enklaste sättet toodetect en aktivitet har körts är toocreate en fil i hello **% TEMP %** mapp när hello aktiviteten lyckas och leta efter den på hello början av hello-aktivitet. Här är ett exempel cmd shell-skript som gör som du.

```cmd
REM   If Task1_Success.txt exists, then Application 1 is already installed.
IF EXIST "%RoleRoot%\Task1_Success.txt" (
  ECHO Application 1 is already installed. Exiting. >> "%TEMP%\StartupLog.txt" 2>&1
  GOTO Finish
)

REM   Run your real exe task
ECHO Running XYZ >> "%TEMP%\StartupLog.txt" 2>&1
"%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (
  REM   hello application installed without error. Create a file tooindicate that hello task
  REM   does not need toobe run again.

  ECHO This line will create a file tooindicate that Application 1 installed correctly. > "%RoleRoot%\Task1_Success.txt"

) ELSE (
  REM   An error occurred. Log hello error and exit with hello error code.

  DATE /T >> "%TEMP%\StartupLog.txt" 2>&1
  TIME /T >> "%TEMP%\StartupLog.txt" 2>&1
  ECHO  An error occurred running task 1. Errorlevel = %ERRORLEVEL%. >> "%TEMP%\StartupLog.txt" 2>&1

  EXIT %ERRORLEVEL%
)

:Finish

REM   Exit normally.
EXIT /B 0
```

## <a name="task-best-practices"></a>Metodtips för aktiviteten
Här följer några rekommendationer som du bör följa när du konfigurerar för din webbplats eller arbetsprocess roll.

### <a name="always-log-startup-activities"></a>Alltid logga Start aktiviteter
Visual Studio tillhandahåller inte en felsökare toostep via batch-filer, så det är bra tooget så mycket data på hello driften av batch-filer som möjligt. Loggning hello utdata från batch-filer, både **stdout** och **stderr**, kan ge viktig information vid toodebug och åtgärda batch-filer. toolog båda **stdout** och **stderr** toohello StartupLog.txt filen i hello directory pekar tooby hello **% TEMP %** miljövariabeln, lägga till hello text `>>  "%TEMP%\\StartupLog.txt" 2>&1`toohello slutet av specifika rader du vill toolog. Till exempel tooexecute setup.exe i hello **% PathToApp1Install** directory:

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

toosimplify XML-data, kan du skapa en wrapper *cmd* -fil som anropar alla programstart uppgifter tillsammans med loggning och garanterar varje underordnad aktivitet resurser hello samma miljövariabler.

Kan du dock irriterande toouse `>> "%TEMP%\StartupLog.txt" 2>&1` på hello slutet av varje startaktivitet. Du kan tillämpa uppgiften loggning genom att skapa en wrapper som hanterar loggning för dig. Den här wrapper anropar hello verkliga batch-fil som du vill toorun. Alla utdata från hello målfilen batch blir omdirigerade toohello *Startuplog.txt* fil.

hello som följande exempel visar hur tooredirect alla utdata från en Startkommandofil. I det här exemplet hello ServerDefinition.csdef skapar en startåtgärd som anropar *logwrap.cmd*. *logwrap.cmd* anrop *Startup2.cmd*, omdirigera alla utdata för**% TEMP %\\StartupLog.txt**.

ServiceDefinition.cmd:

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

**logwrap.cmd:**

```cmd
@ECHO OFF

REM   logwrap.cmd calls passed in batch file, redirecting all output toohello StartupLog.txt log file.

ECHO [%date% %time%] == START logwrap.cmd ============================================== >> "%TEMP%\StartupLog.txt" 2>&1
ECHO [%date% %time%] Running %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Call hello child command batch file, redirecting all output toohello StartupLog.txt log file.
START /B /WAIT %1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   Log hello completion of child command.
ECHO [%date% %time%] Done >> "%TEMP%\StartupLog.txt" 2>&1

IF %ERRORLEVEL% EQU 0 (

   REM   No errors occurred. Exit logwrap.cmd normally.
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B 0

) ELSE (

   REM   Log hello error.
   ECHO [%date% %time%] An error occurred. hello ERRORLEVEL = %ERRORLEVEL%.  >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO [%date% %time%] == END logwrap.cmd ================================================ >> "%TEMP%\StartupLog.txt" 2>&1
   ECHO.  >> "%TEMP%\StartupLog.txt" 2>&1
   EXIT /B %ERRORLEVEL%

)
```

**Startup2.cmd:**

```cmd
@ECHO OFF

REM   This is hello batch file where hello startup steps should be performed. Because of the
REM   way Startup2.cmd was called, all commands and their outputs will be stored in the
REM   StartupLog.txt file in hello directory pointed tooby hello TEMP environment variable.

REM   If an error occurs, hello following command will pass hello ERRORLEVEL back toothe
REM   calling batch file.

ECHO [%date% %time%] Some log information about this task
ECHO [%date% %time%] Some more log information about this task

EXIT %ERRORLEVEL%
```

Exempel på utdata i hello **StartupLog.txt** fil:

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> Hej **StartupLog.txt** filen finns i hello *C:\Resources\temp\\{rollen identifieraren} \RoleTemp* mapp.
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a>Ange executionContext korrekt för start uppgifter
Ange behörigheter för hello startaktivitet. Ibland måste start aktiviteter köras med förhöjda privilegier även om hello rollen körs med normal behörighet.

Hej [executionContext][aktivitet] attribut anger hello behörighetsnivå för hello startaktivitet. Med hjälp av `executionContext="limited"` innebär hello startaktivitet har hello samma behörighetsnivå som hello roll. Med hjälp av `executionContext="elevated"` innebär hello startaktivitet har administratörsbehörighet, vilket gör hello startade uppgiften tooperform administratörsåtgärder utan att ge administratören privilegier tooyour roll.

Ett exempel på en startuppgift som kräver utökade privilegier är en startåtgärd som använder **AppCmd.exe** tooconfigure IIS. **AppCmd.exe** kräver `executionContext="elevated"`.

### <a name="use-hello-appropriate-tasktype"></a>Använd hello lämpliga taskType
Hej [taskType][aktivitet] attributet avgör hello sätt hello startaktivitet körs. Det finns tre värden: **enkel**, **bakgrund**, och **förgrunden**. hello uppgifter i förgrunden och bakgrunden startas asynkront, och sedan hello enkla uppgifter köras synkront en i taget.

Med **enkel** Start uppgifter som du kan ange hello i ordning hello uppgifter köras av hello ordning i vilken hello uppgifter i hello ServiceDefinition.csdef-filen. Om en **enkel** uppgift slutar med en icke-noll slutkod och sedan hello starten avbryts och hello roll startar inte.

Hej skillnaden mellan **bakgrund** Start uppgifter och **förgrunden** Start uppgifter är att **förgrunden** uppgifter hålla hello rollen körs förrän hello  **förgrunden** slutar. Det också innebär att om hello **förgrunden** aktivitet låser sig eller kraschar, hello rollen kommer inte att återanvändas tills hello **förgrunden** aktivitet tvingas stängd. Därför **bakgrund** uppgifter rekommenderas för asynkron Start uppgifter om du inte behöver den funktionen för hello **förgrunden** aktivitet.

### <a name="end-batch-files-with-exit-b-0"></a>End batch-filer med avsluta /B 0
hello rollen börjar endast om hello **errorlevel** från var och en av dina enkla Start uppgift är noll. Inte alla program ange hello **errorlevel** (slutkod) korrekt så hello kommandofilen ska avslutas med ett `EXIT /B 0` om allt körde korrekt.

Det saknas `EXIT /B 0` hello slutet av en Startkommandofil är en vanlig orsak till roller som inte startar.

> [!NOTE]
> Jag har lagt märke till den kapslade batchen filer ibland låser sig när du använder hello `/B` parameter. Du kanske vill du att det här låser sig problemet inte sker om en annan kommandofil anropar den aktuella batchfilen, som om du använder hello toomake [loggen wrapper](#always-log-startup-activities). Du kan hoppa över hello `/B` parameter i det här fallet.
> 
> 

### <a name="expect-startup-tasks-toorun-more-than-once"></a>Förvänta dig Start uppgifter toorun mer än en gång
Inte alla rollen återanvänder inkludera en omstart, men alla rollen återanvänder innehålla alla Start-aktiviteter som körs. Det innebär att start-aktiviteter måste vara kan toorun flera gånger mellan olika omstarter utan problem. Det beskrivs hello [föregående avsnitt](#detect-that-your-task-has-already-run).

### <a name="use-local-storage-toostore-files-that-must-be-accessed-in-hello-role"></a>Använda lokal lagring toostore filer som måste kunna nås i hello roll
Om du vill toocopy eller skapa en fil under startaktivitet som är sedan tillgängliga tooyour roll och sedan filen måste finnas i lokal lagring. Se hello [föregående avsnitt](#create-files-in-local-storage-from-a-startup-task).

## <a name="next-steps"></a>Nästa steg
Granska hello molnet [service model och paket](cloud-services-model-and-package.md)

Läs mer om hur [uppgifter](cloud-services-startup-tasks.md) fungerar.

[Skapa och distribuera](cloud-services-how-to-create-deploy-portal.md) ditt moln-abonnemang.

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[aktivitet]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Startup]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[miljö]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[variabeln]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
[Slutpunkter]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Endpoints
[LocalStorage]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalStorage
[LocalResources]: https://msdn.microsoft.com/library/azure/gg557552.aspx#LocalResources
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
