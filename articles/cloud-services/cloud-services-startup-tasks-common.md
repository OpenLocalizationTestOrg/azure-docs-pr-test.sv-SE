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
# <a name="common-cloud-service-startup-tasks"></a><span data-ttu-id="2c726-103">Vanliga uppgifter för start av Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="2c726-103">Common Cloud Service startup tasks</span></span>
<span data-ttu-id="2c726-104">Den här artikeln innehåller några exempel för vanliga uppgifter för start du tooperform i Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="2c726-104">This article provides some examples of common startup tasks you may want tooperform in your cloud service.</span></span> <span data-ttu-id="2c726-105">Du kan använda Start uppgifter tooperform åtgärder innan du startar en roll.</span><span class="sxs-lookup"><span data-stu-id="2c726-105">You can use startup tasks tooperform operations before a role starts.</span></span> <span data-ttu-id="2c726-106">Åtgärder som du kanske vill tooperform inkluderar installerar en komponent, registrerar COM-komponenter, ange registernycklar eller starta en tidskrävande process.</span><span class="sxs-lookup"><span data-stu-id="2c726-106">Operations that you might want tooperform include installing a component, registering COM components, setting registry keys, or starting a long running process.</span></span> 

<span data-ttu-id="2c726-107">Se [i den här artikeln](cloud-services-startup-tasks.md) toounderstand hur start uppgifter fungerar och hur specifikt toocreate hello poster som definierar en startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c726-107">See [this article](cloud-services-startup-tasks.md) toounderstand how startup tasks work, and specifically how toocreate hello entries that define a startup task.</span></span>

> [!NOTE]
> <span data-ttu-id="2c726-108">Start uppgifter är inte tillämplig tooVirtual datorer, endast tooCloud Service Web- och arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="2c726-108">Startup tasks are not applicable tooVirtual Machines, only tooCloud Service Web and Worker roles.</span></span>
> 

## <a name="define-environment-variables-before-a-role-starts"></a><span data-ttu-id="2c726-109">Definiera miljövariabler innan du startar en roll</span><span class="sxs-lookup"><span data-stu-id="2c726-109">Define environment variables before a role starts</span></span>
<span data-ttu-id="2c726-110">Om du behöver miljövariabler som definieras för en viss aktivitet kan använda hello [miljö] -element inuti hello [aktivitet] element.</span><span class="sxs-lookup"><span data-stu-id="2c726-110">If you need environment variables defined for a specific task, use hello [Environment] element inside hello [Task] element.</span></span>

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

<span data-ttu-id="2c726-111">Variabler kan också använda en [giltig Azure XPath-värdet](cloud-services-role-config-xpath.md) tooreference något om hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="2c726-111">Variables can also use a [valid Azure XPath value](cloud-services-role-config-xpath.md) tooreference something about hello deployment.</span></span> <span data-ttu-id="2c726-112">Istället för att använda hello `value` attribut, definiera en [RoleInstanceValue] underordnat element.</span><span class="sxs-lookup"><span data-stu-id="2c726-112">Instead of using hello `value` attribute, define a [RoleInstanceValue] child element.</span></span>

```xml
<Variable name="PathToStartupStorage">
    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='StartupLocalStorage']/@path" />
</Variable>
```


## <a name="configure-iis-startup-with-appcmdexe"></a><span data-ttu-id="2c726-113">Konfigurera IIS-start med AppCmd.exe</span><span class="sxs-lookup"><span data-stu-id="2c726-113">Configure IIS startup with AppCmd.exe</span></span>
<span data-ttu-id="2c726-114">Hej [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) kommandoradsverktyget kan vara används toomanage IIS-inställningarna vid start på Azure.</span><span class="sxs-lookup"><span data-stu-id="2c726-114">hello [AppCmd.exe](https://technet.microsoft.com/library/jj635852.aspx) command-line tool can be used toomanage IIS settings at startup on Azure.</span></span> <span data-ttu-id="2c726-115">*AppCmd.exe* ger praktiska, kommandoradsverktyg tooconfiguration inställningar för användning i startade uppgifter på Azure.</span><span class="sxs-lookup"><span data-stu-id="2c726-115">*AppCmd.exe* provides convenient, command-line access tooconfiguration settings for use in startup tasks on Azure.</span></span> <span data-ttu-id="2c726-116">Med hjälp av *AppCmd.exe*, Webbplatsinställningar kan läggas till, ändras eller tas bort för program och platser.</span><span class="sxs-lookup"><span data-stu-id="2c726-116">Using *AppCmd.exe*, Website settings can be added, modified, or removed for applications and sites.</span></span>

<span data-ttu-id="2c726-117">Det finns dock några saker toowatch ut för hello används av *AppCmd.exe* som en startåtgärd för:</span><span class="sxs-lookup"><span data-stu-id="2c726-117">However, there are a few things toowatch out for in hello use of *AppCmd.exe* as a startup task:</span></span>

* <span data-ttu-id="2c726-118">Start-uppgifter kan köras mer än en gång mellan olika omstarter.</span><span class="sxs-lookup"><span data-stu-id="2c726-118">Startup tasks can be run more than once between reboots.</span></span> <span data-ttu-id="2c726-119">Till exempel när en roll återanvänds.</span><span class="sxs-lookup"><span data-stu-id="2c726-119">For instance, when a role recycles.</span></span>
* <span data-ttu-id="2c726-120">Om en *AppCmd.exe* åtgärd utförs mer än en gång, kan ett fel genereras.</span><span class="sxs-lookup"><span data-stu-id="2c726-120">If a *AppCmd.exe* action is performed more than once, it may generate an error.</span></span> <span data-ttu-id="2c726-121">Till exempel försöker tooadd ett avsnitt för*Web.config* två gånger kan genererar ett fel.</span><span class="sxs-lookup"><span data-stu-id="2c726-121">For example, attempting tooadd a section too*Web.config* twice could generate an error.</span></span>
* <span data-ttu-id="2c726-122">Start misslyckas om de returnerar slutkoden noll eller **errorlevel**.</span><span class="sxs-lookup"><span data-stu-id="2c726-122">Startup tasks fail if they return a non-zero exit code or **errorlevel**.</span></span> <span data-ttu-id="2c726-123">Till exempel när *AppCmd.exe* genererar ett fel.</span><span class="sxs-lookup"><span data-stu-id="2c726-123">For example, when *AppCmd.exe* generates an error.</span></span>

<span data-ttu-id="2c726-124">Det är en bra idé toocheck hello **errorlevel** efter att *AppCmd.exe*, som är lätt toodo om du omsluter hello-anrop för*AppCmd.exe* med en *cmd.*  fil.</span><span class="sxs-lookup"><span data-stu-id="2c726-124">It is a good practice toocheck hello **errorlevel** after calling *AppCmd.exe*, which is easy toodo if you wrap hello call too*AppCmd.exe* with a *.cmd* file.</span></span> <span data-ttu-id="2c726-125">Om du identifierar en känd **errorlevel** svar, du kan ignorera det eller skicka tillbaka.</span><span class="sxs-lookup"><span data-stu-id="2c726-125">If you detect a known **errorlevel** response, you can ignore it, or pass it back.</span></span>

<span data-ttu-id="2c726-126">Hej errorlevel som returneras av *AppCmd.exe* listas i hello winerror.h fil och kan också visas på [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c726-126">hello errorlevel returned by *AppCmd.exe* are listed in hello winerror.h file, and can also be seen on [MSDN](https://msdn.microsoft.com/library/windows/desktop/ms681382.aspx).</span></span>

### <a name="example-of-managing-hello-error-level"></a><span data-ttu-id="2c726-127">Exempel för att hantera hello Felnivån</span><span class="sxs-lookup"><span data-stu-id="2c726-127">Example of managing hello error level</span></span>
<span data-ttu-id="2c726-128">Det här exemplet lägger till ett avsnitt med komprimering och en komprimering post för JSON-toohello *Web.config* fil med felhantering och loggning.</span><span class="sxs-lookup"><span data-stu-id="2c726-128">This example adds a compression section and a compression entry for JSON toohello *Web.config* file, with error handling and logging.</span></span>

<span data-ttu-id="2c726-129">Hej relevanta avsnitt i hello [ServiceDefinition.csdef] filen visas de här, som innehåller inställningen hello [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) attribut för`elevated` toogive *AppCmd.exe*  tillräckligt toochange hello behörighetsinställningarna för hello *Web.config* fil:</span><span class="sxs-lookup"><span data-stu-id="2c726-129">hello relevant sections of hello [ServiceDefinition.csdef] file are shown here, which include setting hello [executionContext](https://msdn.microsoft.com/library/azure/gg557552.aspx#Task) attribute too`elevated` toogive *AppCmd.exe* sufficient permissions toochange hello settings in hello *Web.config* file:</span></span>

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

<span data-ttu-id="2c726-130">Hej *Startup.cmd* batch-filen använder *AppCmd.exe* tooadd ett avsnitt för komprimering och en komprimering post för JSON-toohello *Web.config* fil.</span><span class="sxs-lookup"><span data-stu-id="2c726-130">hello *Startup.cmd* batch file uses *AppCmd.exe* tooadd a compression section and a compression entry for JSON toohello *Web.config* file.</span></span> <span data-ttu-id="2c726-131">hello förväntat **errorlevel** av 183 anges toozero med hello kontrollera. Exe för kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="2c726-131">hello expected **errorlevel** of 183 is set toozero using hello VERIFY.EXE command-line program.</span></span> <span data-ttu-id="2c726-132">Oväntat errorlevels är loggade tooStartupErrorLog.txt.</span><span class="sxs-lookup"><span data-stu-id="2c726-132">Unexpected errorlevels are logged tooStartupErrorLog.txt.</span></span>

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

## <a name="add-firewall-rules"></a><span data-ttu-id="2c726-133">Lägga till brandväggsregler</span><span class="sxs-lookup"><span data-stu-id="2c726-133">Add firewall rules</span></span>
<span data-ttu-id="2c726-134">I Azure finns effektivt två brandväggar.</span><span class="sxs-lookup"><span data-stu-id="2c726-134">In Azure, there are effectively two firewalls.</span></span> <span data-ttu-id="2c726-135">hello första brandväggen styr anslutningar mellan hello virtuell dator och hello utanför världen.</span><span class="sxs-lookup"><span data-stu-id="2c726-135">hello first firewall controls connections between hello virtual machine and hello outside world.</span></span> <span data-ttu-id="2c726-136">Den här brandväggen styrs av hello [slutpunkter] element i hello [ServiceDefinition.csdef] fil.</span><span class="sxs-lookup"><span data-stu-id="2c726-136">This firewall is controlled by hello [EndPoints] element in hello [ServiceDefinition.csdef] file.</span></span>

<span data-ttu-id="2c726-137">andra hello-brandväggen styr anslutningar mellan hello virtuell dator och hello processer i den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="2c726-137">hello second firewall controls connections between hello virtual machine and hello processes within that virtual machine.</span></span> <span data-ttu-id="2c726-138">Den här brandväggen kan styras efter hello `netsh advfirewall firewall` kommandoradsverktyget.</span><span class="sxs-lookup"><span data-stu-id="2c726-138">This firewall can be controlled by hello `netsh advfirewall firewall` command-line tool.</span></span>

<span data-ttu-id="2c726-139">Azure skapar brandväggsregler för hello processer som startas inom dina roller.</span><span class="sxs-lookup"><span data-stu-id="2c726-139">Azure creates firewall rules for hello processes started within your roles.</span></span> <span data-ttu-id="2c726-140">Till exempel när du startar en tjänst eller ett program, skapas Azure automatiskt hello nödvändiga brandväggen regler tooallow som tjänsten toocommunicate med hello Internet.</span><span class="sxs-lookup"><span data-stu-id="2c726-140">For example, when you start a service or program, Azure automatically creates hello necessary firewall rules tooallow that service toocommunicate with hello Internet.</span></span> <span data-ttu-id="2c726-141">Om du skapar en tjänst som startas av en process utanför din roll (till exempel en COM +-tjänst eller en schemalagd aktivitet i Windows), måste du dock toomanually skapa en regel tooallow åtkomst toothat brandväggen.</span><span class="sxs-lookup"><span data-stu-id="2c726-141">However, if you create a service that is started by a process outside your role (like a COM+ service or a Windows Scheduled Task), you need toomanually create a firewall rule tooallow access toothat service.</span></span> <span data-ttu-id="2c726-142">Brandväggsreglerna kan skapas med hjälp av en startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c726-142">These firewall rules can be created by using a startup task.</span></span>

<span data-ttu-id="2c726-143">En startåtgärd som skapar en brandväggsregel måste ha en [executionContext][aktivitet] av **utökade**.</span><span class="sxs-lookup"><span data-stu-id="2c726-143">A startup task that creates a firewall rule must have an [executionContext][Task] of **elevated**.</span></span> <span data-ttu-id="2c726-144">Lägg till följande startade uppgiften toohello hello [ServiceDefinition.csdef] fil.</span><span class="sxs-lookup"><span data-stu-id="2c726-144">Add hello following startup task toohello [ServiceDefinition.csdef] file.</span></span>

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

<span data-ttu-id="2c726-145">tooadd hello brandväggsregeln, måste du använda hello lämpliga `netsh advfirewall firewall` kommandon i kommandofilen startades.</span><span class="sxs-lookup"><span data-stu-id="2c726-145">tooadd hello firewall rule, you must use hello appropriate `netsh advfirewall firewall` commands in your startup batch file.</span></span> <span data-ttu-id="2c726-146">I det här exemplet kräver hello startaktivitet säkerhet och kryptering för TCP-port 80.</span><span class="sxs-lookup"><span data-stu-id="2c726-146">In this example, hello startup task requires security and encryption for TCP port 80.</span></span>

```cmd
REM   Add a firewall rule in a startup task.

REM   Add an inbound rule requiring security and encryption for TCP port 80 traffic.
netsh advfirewall firewall add rule name="Require Encryption for Inbound TCP/80" protocol=TCP dir=in localport=80 security=authdynenc action=allow >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

## <a name="block-a-specific-ip-address"></a><span data-ttu-id="2c726-147">Blockera en specifik IP-adress</span><span class="sxs-lookup"><span data-stu-id="2c726-147">Block a specific IP address</span></span>
<span data-ttu-id="2c726-148">Du kan begränsa en Azure web roll åtkomst tooa uppsättning angivna IP-adresser genom att ändra din IIS **web.config** fil.</span><span class="sxs-lookup"><span data-stu-id="2c726-148">You can restrict an Azure web role access tooa set of specified IP addresses by modifying your IIS **web.config** file.</span></span> <span data-ttu-id="2c726-149">Du måste också toouse kommandofilen som låser upp hello **ipSecurity** avsnitt i hello **ApplicationHost.config** fil.</span><span class="sxs-lookup"><span data-stu-id="2c726-149">You also need toouse a command file which unlocks hello **ipSecurity** section of hello **ApplicationHost.config** file.</span></span>

<span data-ttu-id="2c726-150">toodo låsa upp hello **ipSecurity** avsnitt i hello **ApplicationHost.config** fil, skapa en kommandofil som körs vid start av rollen.</span><span class="sxs-lookup"><span data-stu-id="2c726-150">toodo unlock hello **ipSecurity** section of hello **ApplicationHost.config** file, create a command file that runs at role start.</span></span> <span data-ttu-id="2c726-151">Skapa en mapp på rotnivå för hello web rollen kallas **Start** och skapa en batchfil som anropas i den här mappen **startup.cmd**.</span><span class="sxs-lookup"><span data-stu-id="2c726-151">Create a folder at hello root level of your web role called **startup** and, within this folder, create a batch file called **startup.cmd**.</span></span> <span data-ttu-id="2c726-152">Lägg till den här filen tooyour Visual Studio-projekt och ange hello egenskaper för**Kopiera alltid** tooensure den ingår i paketet.</span><span class="sxs-lookup"><span data-stu-id="2c726-152">Add this file tooyour Visual Studio project and set hello properties too**Copy Always** tooensure that it is included in your package.</span></span>

<span data-ttu-id="2c726-153">Lägg till följande startade uppgiften toohello hello [ServiceDefinition.csdef] fil.</span><span class="sxs-lookup"><span data-stu-id="2c726-153">Add hello following startup task toohello [ServiceDefinition.csdef] file.</span></span>

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

<span data-ttu-id="2c726-154">Lägg till det här kommandot toohello **startup.cmd** fil:</span><span class="sxs-lookup"><span data-stu-id="2c726-154">Add this command toohello **startup.cmd** file:</span></span>

```cmd
@echo off
@echo Installing "IPv4 Address and Domain Restrictions" feature 
powershell -ExecutionPolicy Unrestricted -command "Install-WindowsFeature Web-IP-Security"
@echo Unlocking configuration for "IPv4 Address and Domain Restrictions" feature 
%windir%\system32\inetsrv\AppCmd.exe unlock config -section:system.webServer/security/ipSecurity
```

<span data-ttu-id="2c726-155">Uppgiften gör hello **startup.cmd** batch-filen toobe körs varje gång hello webbroll har initierats säkerställer att hello krävs **ipSecurity** avsnitt är olåst.</span><span class="sxs-lookup"><span data-stu-id="2c726-155">This task causes hello **startup.cmd** batch file toobe run every time hello web role is initialized, ensuring that hello required **ipSecurity** section is unlocked.</span></span>

<span data-ttu-id="2c726-156">Slutligen ändrar hello [system.webServer avsnittet](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) din webbroll **web.config** filen tooadd en lista över IP-adresser som beviljas åtkomst till, enligt följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="2c726-156">Finally, modify hello [system.webServer section](http://www.iis.net/configreference/system.webserver/security/ipsecurity#005) your web role’s **web.config** file tooadd a list of IP addresses that are granted access, as shown in hello following example:</span></span>

<span data-ttu-id="2c726-157">Det här exemplet config **kan** alla IP-adresser tooaccess hello server förutom hello två definierats</span><span class="sxs-lookup"><span data-stu-id="2c726-157">This sample config **allows** all IPs tooaccess hello server except hello two defined</span></span>

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

<span data-ttu-id="2c726-158">Det här exemplet config **nekar** alla IP-adresser från att komma åt hello server förutom hello två definierats.</span><span class="sxs-lookup"><span data-stu-id="2c726-158">This sample config **denies** all IPs from accessing hello server except for hello two defined.</span></span>

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

## <a name="create-a-powershell-startup-task"></a><span data-ttu-id="2c726-159">Skapa en startåtgärd för PowerShell</span><span class="sxs-lookup"><span data-stu-id="2c726-159">Create a PowerShell startup task</span></span>
<span data-ttu-id="2c726-160">Windows PowerShell-skript kan inte anropas direkt från hello [ServiceDefinition.csdef] -fil, men de kan anropas från inom en Startkommandofil.</span><span class="sxs-lookup"><span data-stu-id="2c726-160">Windows PowerShell scripts cannot be called directly from hello [ServiceDefinition.csdef] file, but they can be invoked from within a startup batch file.</span></span>

<span data-ttu-id="2c726-161">Osignerade skript körs inte i PowerShell (som standard).</span><span class="sxs-lookup"><span data-stu-id="2c726-161">PowerShell (by default) does not run unsigned scripts.</span></span> <span data-ttu-id="2c726-162">Om du inte loggar skriptet, behöver du tooconfigure PowerShell toorun osignerade skript.</span><span class="sxs-lookup"><span data-stu-id="2c726-162">Unless you sign your script, you need tooconfigure PowerShell toorun unsigned scripts.</span></span> <span data-ttu-id="2c726-163">toorun osignerade skript, hello **ExecutionPolicy** måste anges för**obegränsat**.</span><span class="sxs-lookup"><span data-stu-id="2c726-163">toorun unsigned scripts, hello **ExecutionPolicy** must be set too**Unrestricted**.</span></span> <span data-ttu-id="2c726-164">Hej **ExecutionPolicy** inställning som du Använd baseras på hello version av Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2c726-164">hello **ExecutionPolicy** setting that you use is based on hello version of Windows PowerShell.</span></span>

```cmd
REM   Run an unsigned PowerShell script and log hello output
PowerShell -ExecutionPolicy Unrestricted .\startup.ps1 >> "%TEMP%\StartupLog.txt" 2>&1

REM   If an error occurred, return hello errorlevel.
EXIT /B %errorlevel%
```

<span data-ttu-id="2c726-165">Om du använder ett Gästoperativsystem som kör PowerShell 2.0 eller 1.0 du kan tvinga version 2 toorun, och om det är inte tillgänglig, kan du använda version 1.</span><span class="sxs-lookup"><span data-stu-id="2c726-165">If you're using a Guest OS that is runs PowerShell 2.0 or 1.0 you can force version 2 toorun, and if unavailable, use version 1.</span></span>

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

## <a name="create-files-in-local-storage-from-a-startup-task"></a><span data-ttu-id="2c726-166">Skapa filer i lokal lagring från en startåtgärd</span><span class="sxs-lookup"><span data-stu-id="2c726-166">Create files in local storage from a startup task</span></span>
<span data-ttu-id="2c726-167">Du kan använda en lokal lagring resurs toostore filer som skapas av din startaktivitet som senare används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="2c726-167">You can use a local storage resource toostore files created by your startup task that is accessed later by your application.</span></span>

<span data-ttu-id="2c726-168">toocreate hello lokal lagringsresurs, lägga till en [LocalResources] avsnittet toohello [ServiceDefinition.csdef] filen och Lägg sedan till hello [LocalStorage] underordnat element.</span><span class="sxs-lookup"><span data-stu-id="2c726-168">toocreate hello local storage resource, add a [LocalResources] section toohello [ServiceDefinition.csdef] file and then add hello [LocalStorage] child element.</span></span> <span data-ttu-id="2c726-169">Ge hello lokal lagringsresurs ett unikt namn och en lämplig storlek för startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c726-169">Give hello local storage resource a unique name and an appropriate size for your startup task.</span></span>

<span data-ttu-id="2c726-170">toouse en resurs för lokal lagring i ditt startaktivitet måste toocreate resursplats en miljö variabeln tooreference hello lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="2c726-170">toouse a local storage resource in your startup task, you need toocreate an environment variable tooreference hello local storage resource location.</span></span> <span data-ttu-id="2c726-171">Hej startaktivitet och hello program kan tooread och skriva filer toohello lokal lagringsresurs.</span><span class="sxs-lookup"><span data-stu-id="2c726-171">Then hello startup task and hello application are able tooread and write files toohello local storage resource.</span></span>

<span data-ttu-id="2c726-172">Hej relevanta avsnitt i hello **ServiceDefinition.csdef** filen visas här:</span><span class="sxs-lookup"><span data-stu-id="2c726-172">hello relevant sections of hello **ServiceDefinition.csdef** file are shown here:</span></span>

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

<span data-ttu-id="2c726-173">Exempelvis detta **Startup.cmd** kommandofilen använder hello **PathToStartupStorage** miljö variabeln toocreate hello filen **MyTest.txt** på hello lokal lagring plats.</span><span class="sxs-lookup"><span data-stu-id="2c726-173">As an example, this **Startup.cmd** batch file uses hello **PathToStartupStorage** environment variable toocreate hello file **MyTest.txt** on hello local storage location.</span></span>

```cmd
REM   Create a simple text file.

ECHO This text will go into hello MyTest.txt file which will be in hello    >  "%PathToStartupStorage%\MyTest.txt"
ECHO path pointed tooby hello PathToStartupStorage environment variable.  >> "%PathToStartupStorage%\MyTest.txt"
ECHO hello contents of hello PathToStartupStorage environment variable is   >> "%PathToStartupStorage%\MyTest.txt"
ECHO "%PathToStartupStorage%".                                          >> "%PathToStartupStorage%\MyTest.txt"

REM   Exit hello batch file with ERRORLEVEL 0.

EXIT /b 0
```

<span data-ttu-id="2c726-174">Du kan komma åt mappen för lokal lagring från hello Azure SDK med hjälp av hello [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) metod.</span><span class="sxs-lookup"><span data-stu-id="2c726-174">You can access local storage folder from hello Azure SDK by using hello [GetLocalResource](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) method.</span></span>

```csharp
string localStoragePath = Microsoft.WindowsAzure.ServiceRuntime.RoleEnvironment.GetLocalResource("StartupLocalStorage").RootPath;

string fileContent = System.IO.File.ReadAllText(System.IO.Path.Combine(localStoragePath, "MyTestFile.txt"));
```

## <a name="run-in-hello-emulator-or-cloud"></a><span data-ttu-id="2c726-175">Kör i hello emulatorn eller molnet</span><span class="sxs-lookup"><span data-stu-id="2c726-175">Run in hello emulator or cloud</span></span>
<span data-ttu-id="2c726-176">Du kan ha din startaktivitet som utför olika steg när den körs i hello molnet jämfört med toowhen i hello beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="2c726-176">You can have your startup task perform different steps when it is operating in hello cloud compared toowhen it is in hello compute emulator.</span></span> <span data-ttu-id="2c726-177">Exempelvis kanske toouse en ny kopia av SQL-data endast när den körs i hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="2c726-177">For example, you may want toouse a fresh copy of your SQL data only when running in hello emulator.</span></span> <span data-ttu-id="2c726-178">Eller vill du kanske toodo prestandaoptimeringar för hello moln som du inte behöver toodo vid körning i hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="2c726-178">Or you may want toodo some performance optimizations for hello cloud that you don't need toodo when running in hello emulator.</span></span>

<span data-ttu-id="2c726-179">Den här möjligheten tooperform olika åtgärder på hello compute emulator och hello molnet kan åstadkommas genom att skapa en miljövariabel i hello [ServiceDefinition.csdef] fil.</span><span class="sxs-lookup"><span data-stu-id="2c726-179">This ability tooperform different actions on hello compute emulator and hello cloud can be accomplished by creating an environment variable in hello [ServiceDefinition.csdef] file.</span></span> <span data-ttu-id="2c726-180">Testa att miljövariabeln för ett värde i din startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c726-180">You then test that environment variable for a value in your startup task.</span></span>

<span data-ttu-id="2c726-181">toocreate hello miljövariabeln, lägga till hello [variabeln]/[RoleInstanceValue] element och skapa en XPath-värdet för `/RoleEnvironment/Deployment/@emulated`.</span><span class="sxs-lookup"><span data-stu-id="2c726-181">toocreate hello environment variable, add hello [Variable]/[RoleInstanceValue] element and create an XPath value of `/RoleEnvironment/Deployment/@emulated`.</span></span> <span data-ttu-id="2c726-182">Hej värdet för hello **ComputeEmulatorRunning %** miljövariabel är `true` vid körning på hello beräkningsemulatorn och `false` när du kör på hello moln.</span><span class="sxs-lookup"><span data-stu-id="2c726-182">hello value of hello **%ComputeEmulatorRunning%** environment variable is `true` when running on hello compute emulator, and `false` when running on hello cloud.</span></span>

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

<span data-ttu-id="2c726-183">hello aktivitet kan nu kontrollera hello **ComputeEmulatorRunning %** miljö variabeln tooperform olika åtgärder baserat på om hello rollen körs i hello molnet eller hello emulatorn.</span><span class="sxs-lookup"><span data-stu-id="2c726-183">hello task can now check hello **%ComputeEmulatorRunning%** environment variable tooperform different actions based on whether hello role is running in hello cloud or hello emulator.</span></span> <span data-ttu-id="2c726-184">Här är ett cmd shell-skript som söker efter att miljövariabeln.</span><span class="sxs-lookup"><span data-stu-id="2c726-184">Here is a .cmd shell script that checks for that environment variable.</span></span>

```cmd
REM   Check if this task is running on hello compute emulator.

IF "%ComputeEmulatorRunning%" == "true" (
    REM   This task is running on hello compute emulator. Perform tasks that must be run only in hello compute emulator.
) ELSE (
    REM   This task is running on hello cloud. Perform tasks that must be run only in hello cloud.
)
```


## <a name="detect-that-your-task-has-already-run"></a><span data-ttu-id="2c726-185">Identifiera uppgiften har körts</span><span class="sxs-lookup"><span data-stu-id="2c726-185">Detect that your task has already run</span></span>
<span data-ttu-id="2c726-186">hello-rollen kan återvinna utan en omstart orsakar din start uppgifter toorun igen.</span><span class="sxs-lookup"><span data-stu-id="2c726-186">hello role may recycle without a reboot causing your startup tasks toorun again.</span></span> <span data-ttu-id="2c726-187">Det finns inga flaggan tooindicate som en aktivitet har redan körs på hello som värd för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2c726-187">There is no flag tooindicate that a task has already run on hello hosting VM.</span></span> <span data-ttu-id="2c726-188">Du kan ha vissa uppgifter där det spelar ingen roll de köras flera gånger.</span><span class="sxs-lookup"><span data-stu-id="2c726-188">You may have some tasks where it doesn't matter that they run multiple times.</span></span> <span data-ttu-id="2c726-189">Men kan du köra i en situation där du behöver tooprevent en aktivitet körs mer än en gång.</span><span class="sxs-lookup"><span data-stu-id="2c726-189">However, you may run into a situation where you need tooprevent a task from running more than once.</span></span>

<span data-ttu-id="2c726-190">hello enklaste sättet toodetect en aktivitet har körts är toocreate en fil i hello **% TEMP %** mapp när hello aktiviteten lyckas och leta efter den på hello början av hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c726-190">hello simplest way toodetect that a task has already run is toocreate a file in hello **%TEMP%** folder when hello task is successful and look for it at hello start of hello task.</span></span> <span data-ttu-id="2c726-191">Här är ett exempel cmd shell-skript som gör som du.</span><span class="sxs-lookup"><span data-stu-id="2c726-191">Here is a sample cmd shell script that does that for you.</span></span>

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

## <a name="task-best-practices"></a><span data-ttu-id="2c726-192">Metodtips för aktiviteten</span><span class="sxs-lookup"><span data-stu-id="2c726-192">Task best practices</span></span>
<span data-ttu-id="2c726-193">Här följer några rekommendationer som du bör följa när du konfigurerar för din webbplats eller arbetsprocess roll.</span><span class="sxs-lookup"><span data-stu-id="2c726-193">Here are some best practices you should follow when configuring task for your web or worker role.</span></span>

### <a name="always-log-startup-activities"></a><span data-ttu-id="2c726-194">Alltid logga Start aktiviteter</span><span class="sxs-lookup"><span data-stu-id="2c726-194">Always log startup activities</span></span>
<span data-ttu-id="2c726-195">Visual Studio tillhandahåller inte en felsökare toostep via batch-filer, så det är bra tooget så mycket data på hello driften av batch-filer som möjligt.</span><span class="sxs-lookup"><span data-stu-id="2c726-195">Visual Studio does not provide a debugger toostep through batch files, so it's good tooget as much data on hello operation of batch files as possible.</span></span> <span data-ttu-id="2c726-196">Loggning hello utdata från batch-filer, både **stdout** och **stderr**, kan ge viktig information vid toodebug och åtgärda batch-filer.</span><span class="sxs-lookup"><span data-stu-id="2c726-196">Logging hello output of batch files, both **stdout** and **stderr**, can give you important information when trying toodebug and fix batch files.</span></span> <span data-ttu-id="2c726-197">toolog båda **stdout** och **stderr** toohello StartupLog.txt filen i hello directory pekar tooby hello **% TEMP %** miljövariabeln, lägga till hello text `>>  "%TEMP%\\StartupLog.txt" 2>&1`toohello slutet av specifika rader du vill toolog.</span><span class="sxs-lookup"><span data-stu-id="2c726-197">toolog both **stdout** and **stderr** toohello StartupLog.txt file in hello directory pointed tooby hello **%TEMP%** environment variable, add hello text `>>  "%TEMP%\\StartupLog.txt" 2>&1` toohello end of specific lines you want toolog.</span></span> <span data-ttu-id="2c726-198">Till exempel tooexecute setup.exe i hello **% PathToApp1Install** directory:</span><span class="sxs-lookup"><span data-stu-id="2c726-198">For example, tooexecute setup.exe in hello **%PathToApp1Install%** directory:</span></span>

    "%PathToApp1Install%\setup.exe" >> "%TEMP%\StartupLog.txt" 2>&1

<span data-ttu-id="2c726-199">toosimplify XML-data, kan du skapa en wrapper *cmd* -fil som anropar alla programstart uppgifter tillsammans med loggning och garanterar varje underordnad aktivitet resurser hello samma miljövariabler.</span><span class="sxs-lookup"><span data-stu-id="2c726-199">toosimplify your xml, you can create a wrapper *cmd* file that calls all of your startup tasks along with logging and ensures each child-task shares hello same environment variables.</span></span>

<span data-ttu-id="2c726-200">Kan du dock irriterande toouse `>> "%TEMP%\StartupLog.txt" 2>&1` på hello slutet av varje startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c726-200">You may find it annoying though toouse `>> "%TEMP%\StartupLog.txt" 2>&1` on hello end of each startup task.</span></span> <span data-ttu-id="2c726-201">Du kan tillämpa uppgiften loggning genom att skapa en wrapper som hanterar loggning för dig.</span><span class="sxs-lookup"><span data-stu-id="2c726-201">You can enforce task logging by creating a wrapper that handles logging for you.</span></span> <span data-ttu-id="2c726-202">Den här wrapper anropar hello verkliga batch-fil som du vill toorun.</span><span class="sxs-lookup"><span data-stu-id="2c726-202">This wrapper calls hello real batch file you want toorun.</span></span> <span data-ttu-id="2c726-203">Alla utdata från hello målfilen batch blir omdirigerade toohello *Startuplog.txt* fil.</span><span class="sxs-lookup"><span data-stu-id="2c726-203">Any output from hello target batch file will be redirected toohello *Startuplog.txt* file.</span></span>

<span data-ttu-id="2c726-204">hello som följande exempel visar hur tooredirect alla utdata från en Startkommandofil.</span><span class="sxs-lookup"><span data-stu-id="2c726-204">hello following example shows how tooredirect all output from a startup batch file.</span></span> <span data-ttu-id="2c726-205">I det här exemplet hello ServerDefinition.csdef skapar en startåtgärd som anropar *logwrap.cmd*.</span><span class="sxs-lookup"><span data-stu-id="2c726-205">In this example, hello ServerDefinition.csdef file creates a startup task that calls *logwrap.cmd*.</span></span> <span data-ttu-id="2c726-206">*logwrap.cmd* anrop *Startup2.cmd*, omdirigera alla utdata för**% TEMP %\\StartupLog.txt**.</span><span class="sxs-lookup"><span data-stu-id="2c726-206">*logwrap.cmd* calls *Startup2.cmd*, redirecting all output too**%TEMP%\\StartupLog.txt**.</span></span>

<span data-ttu-id="2c726-207">ServiceDefinition.cmd:</span><span class="sxs-lookup"><span data-stu-id="2c726-207">ServiceDefinition.cmd:</span></span>

```xml
<Startup>
    <Task commandLine="logwrap.cmd startup2.cmd" executionContext="limited" taskType="simple" />
</Startup>
```

<span data-ttu-id="2c726-208">**logwrap.cmd:**</span><span class="sxs-lookup"><span data-stu-id="2c726-208">**logwrap.cmd:**</span></span>

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

<span data-ttu-id="2c726-209">**Startup2.cmd:**</span><span class="sxs-lookup"><span data-stu-id="2c726-209">**Startup2.cmd:**</span></span>

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

<span data-ttu-id="2c726-210">Exempel på utdata i hello **StartupLog.txt** fil:</span><span class="sxs-lookup"><span data-stu-id="2c726-210">Sample output in hello **StartupLog.txt** file:</span></span>

```txt
[Mon 10/17/2016 20:24:46.75] == START logwrap.cmd ============================================== 
[Mon 10/17/2016 20:24:46.75] Running command1.cmd 
[Mon 10/17/2016 20:24:46.77] Some log information about this task
[Mon 10/17/2016 20:24:46.77] Some more log information about this task
[Mon 10/17/2016 20:24:46.77] Done 
[Mon 10/17/2016 20:24:46.77] == END logwrap.cmd ================================================ 
```

> [!TIP]
> <span data-ttu-id="2c726-211">Hej **StartupLog.txt** filen finns i hello *C:\Resources\temp\\{rollen identifieraren} \RoleTemp* mapp.</span><span class="sxs-lookup"><span data-stu-id="2c726-211">hello **StartupLog.txt** file is located in hello *C:\Resources\temp\\{role identifier}\RoleTemp* folder.</span></span>
> 
> 

### <a name="set-executioncontext-appropriately-for-startup-tasks"></a><span data-ttu-id="2c726-212">Ange executionContext korrekt för start uppgifter</span><span class="sxs-lookup"><span data-stu-id="2c726-212">Set executionContext appropriately for startup tasks</span></span>
<span data-ttu-id="2c726-213">Ange behörigheter för hello startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c726-213">Set privileges appropriately for hello startup task.</span></span> <span data-ttu-id="2c726-214">Ibland måste start aktiviteter köras med förhöjda privilegier även om hello rollen körs med normal behörighet.</span><span class="sxs-lookup"><span data-stu-id="2c726-214">Sometimes startup tasks must run with elevated privileges even though hello role runs with normal privileges.</span></span>

<span data-ttu-id="2c726-215">Hej [executionContext][aktivitet] attribut anger hello behörighetsnivå för hello startaktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c726-215">hello [executionContext][Task] attribute sets hello privilege level of hello startup task.</span></span> <span data-ttu-id="2c726-216">Med hjälp av `executionContext="limited"` innebär hello startaktivitet har hello samma behörighetsnivå som hello roll.</span><span class="sxs-lookup"><span data-stu-id="2c726-216">Using `executionContext="limited"` means hello startup task has hello same privilege level as hello role.</span></span> <span data-ttu-id="2c726-217">Med hjälp av `executionContext="elevated"` innebär hello startaktivitet har administratörsbehörighet, vilket gör hello startade uppgiften tooperform administratörsåtgärder utan att ge administratören privilegier tooyour roll.</span><span class="sxs-lookup"><span data-stu-id="2c726-217">Using `executionContext="elevated"` means hello startup task has administrator privileges, which allows hello startup task tooperform administrator tasks without giving administrator privileges tooyour role.</span></span>

<span data-ttu-id="2c726-218">Ett exempel på en startuppgift som kräver utökade privilegier är en startåtgärd som använder **AppCmd.exe** tooconfigure IIS.</span><span class="sxs-lookup"><span data-stu-id="2c726-218">An example of a startup task that requires elevated privileges is a startup task that uses **AppCmd.exe** tooconfigure IIS.</span></span> <span data-ttu-id="2c726-219">**AppCmd.exe** kräver `executionContext="elevated"`.</span><span class="sxs-lookup"><span data-stu-id="2c726-219">**AppCmd.exe** requires `executionContext="elevated"`.</span></span>

### <a name="use-hello-appropriate-tasktype"></a><span data-ttu-id="2c726-220">Använd hello lämpliga taskType</span><span class="sxs-lookup"><span data-stu-id="2c726-220">Use hello appropriate taskType</span></span>
<span data-ttu-id="2c726-221">Hej [taskType][aktivitet] attributet avgör hello sätt hello startaktivitet körs.</span><span class="sxs-lookup"><span data-stu-id="2c726-221">hello [taskType][Task] attribute determines hello way hello startup task is executed.</span></span> <span data-ttu-id="2c726-222">Det finns tre värden: **enkel**, **bakgrund**, och **förgrunden**.</span><span class="sxs-lookup"><span data-stu-id="2c726-222">There are three values: **simple**, **background**, and **foreground**.</span></span> <span data-ttu-id="2c726-223">hello uppgifter i förgrunden och bakgrunden startas asynkront, och sedan hello enkla uppgifter köras synkront en i taget.</span><span class="sxs-lookup"><span data-stu-id="2c726-223">hello background and foreground tasks are started asynchronously, and then hello simple tasks are executed synchronously one at a time.</span></span>

<span data-ttu-id="2c726-224">Med **enkel** Start uppgifter som du kan ange hello i ordning hello uppgifter köras av hello ordning i vilken hello uppgifter i hello ServiceDefinition.csdef-filen.</span><span class="sxs-lookup"><span data-stu-id="2c726-224">With **simple** startup tasks, you can set hello order in which hello tasks run by hello order in which hello tasks are listed in hello ServiceDefinition.csdef file.</span></span> <span data-ttu-id="2c726-225">Om en **enkel** uppgift slutar med en icke-noll slutkod och sedan hello starten avbryts och hello roll startar inte.</span><span class="sxs-lookup"><span data-stu-id="2c726-225">If a **simple** task ends with a non-zero exit code, then hello startup procedure stops and hello role does not start.</span></span>

<span data-ttu-id="2c726-226">Hej skillnaden mellan **bakgrund** Start uppgifter och **förgrunden** Start uppgifter är att **förgrunden** uppgifter hålla hello rollen körs förrän hello  **förgrunden** slutar.</span><span class="sxs-lookup"><span data-stu-id="2c726-226">hello difference between **background** startup tasks and **foreground** startup tasks is that **foreground** tasks keep hello role running until hello **foreground** task ends.</span></span> <span data-ttu-id="2c726-227">Det också innebär att om hello **förgrunden** aktivitet låser sig eller kraschar, hello rollen kommer inte att återanvändas tills hello **förgrunden** aktivitet tvingas stängd.</span><span class="sxs-lookup"><span data-stu-id="2c726-227">This also means that if hello **foreground** task hangs or crashes, hello role will not recycle until hello **foreground** task is forced closed.</span></span> <span data-ttu-id="2c726-228">Därför **bakgrund** uppgifter rekommenderas för asynkron Start uppgifter om du inte behöver den funktionen för hello **förgrunden** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="2c726-228">For this reason, **background** tasks are recommended for asynchronous startup tasks unless you need that feature of hello **foreground** task.</span></span>

### <a name="end-batch-files-with-exit-b-0"></a><span data-ttu-id="2c726-229">End batch-filer med avsluta /B 0</span><span class="sxs-lookup"><span data-stu-id="2c726-229">End batch files with EXIT /B 0</span></span>
<span data-ttu-id="2c726-230">hello rollen börjar endast om hello **errorlevel** från var och en av dina enkla Start uppgift är noll.</span><span class="sxs-lookup"><span data-stu-id="2c726-230">hello role will only start if hello **errorlevel** from each of your simple startup task is zero.</span></span> <span data-ttu-id="2c726-231">Inte alla program ange hello **errorlevel** (slutkod) korrekt så hello kommandofilen ska avslutas med ett `EXIT /B 0` om allt körde korrekt.</span><span class="sxs-lookup"><span data-stu-id="2c726-231">Not all programs set hello **errorlevel** (exit code) correctly, so hello batch file should end with an `EXIT /B 0` if everything ran correctly.</span></span>

<span data-ttu-id="2c726-232">Det saknas `EXIT /B 0` hello slutet av en Startkommandofil är en vanlig orsak till roller som inte startar.</span><span class="sxs-lookup"><span data-stu-id="2c726-232">A missing `EXIT /B 0` at hello end of a startup batch file is a common cause of roles that do not start.</span></span>

> [!NOTE]
> <span data-ttu-id="2c726-233">Jag har lagt märke till den kapslade batchen filer ibland låser sig när du använder hello `/B` parameter.</span><span class="sxs-lookup"><span data-stu-id="2c726-233">I've noticed that nested batch files sometimes hang when using hello `/B` parameter.</span></span> <span data-ttu-id="2c726-234">Du kanske vill du att det här låser sig problemet inte sker om en annan kommandofil anropar den aktuella batchfilen, som om du använder hello toomake [loggen wrapper](#always-log-startup-activities).</span><span class="sxs-lookup"><span data-stu-id="2c726-234">You may want toomake sure that this hang problem does not happen if another batch file calls your current batch file, like if you use hello [log wrapper](#always-log-startup-activities).</span></span> <span data-ttu-id="2c726-235">Du kan hoppa över hello `/B` parameter i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="2c726-235">You can omit hello `/B` parameter in this case.</span></span>
> 
> 

### <a name="expect-startup-tasks-toorun-more-than-once"></a><span data-ttu-id="2c726-236">Förvänta dig Start uppgifter toorun mer än en gång</span><span class="sxs-lookup"><span data-stu-id="2c726-236">Expect startup tasks toorun more than once</span></span>
<span data-ttu-id="2c726-237">Inte alla rollen återanvänder inkludera en omstart, men alla rollen återanvänder innehålla alla Start-aktiviteter som körs.</span><span class="sxs-lookup"><span data-stu-id="2c726-237">Not all role recycles include a reboot, but all role recycles include running all startup tasks.</span></span> <span data-ttu-id="2c726-238">Det innebär att start-aktiviteter måste vara kan toorun flera gånger mellan olika omstarter utan problem.</span><span class="sxs-lookup"><span data-stu-id="2c726-238">This means that startup tasks must be able toorun multiple times between reboots without any problems.</span></span> <span data-ttu-id="2c726-239">Det beskrivs hello [föregående avsnitt](#detect-that-your-task-has-already-run).</span><span class="sxs-lookup"><span data-stu-id="2c726-239">This is discussed in hello [preceding section](#detect-that-your-task-has-already-run).</span></span>

### <a name="use-local-storage-toostore-files-that-must-be-accessed-in-hello-role"></a><span data-ttu-id="2c726-240">Använda lokal lagring toostore filer som måste kunna nås i hello roll</span><span class="sxs-lookup"><span data-stu-id="2c726-240">Use local storage toostore files that must be accessed in hello role</span></span>
<span data-ttu-id="2c726-241">Om du vill toocopy eller skapa en fil under startaktivitet som är sedan tillgängliga tooyour roll och sedan filen måste finnas i lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="2c726-241">If you want toocopy or create a file during your startup task that is then accessible tooyour role, then that file must be placed in local storage.</span></span> <span data-ttu-id="2c726-242">Se hello [föregående avsnitt](#create-files-in-local-storage-from-a-startup-task).</span><span class="sxs-lookup"><span data-stu-id="2c726-242">See hello [preceding section](#create-files-in-local-storage-from-a-startup-task).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c726-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c726-243">Next steps</span></span>
<span data-ttu-id="2c726-244">Granska hello molnet [service model och paket](cloud-services-model-and-package.md)</span><span class="sxs-lookup"><span data-stu-id="2c726-244">Review hello cloud [service model and package](cloud-services-model-and-package.md)</span></span>

<span data-ttu-id="2c726-245">Läs mer om hur [uppgifter](cloud-services-startup-tasks.md) fungerar.</span><span class="sxs-lookup"><span data-stu-id="2c726-245">Learn more about how [Tasks](cloud-services-startup-tasks.md) work.</span></span>

<span data-ttu-id="2c726-246">[Skapa och distribuera](cloud-services-how-to-create-deploy-portal.md) ditt moln-abonnemang.</span><span class="sxs-lookup"><span data-stu-id="2c726-246">[Create and deploy](cloud-services-how-to-create-deploy-portal.md) your cloud service package.</span></span>

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
