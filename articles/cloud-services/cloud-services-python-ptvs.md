---
title: "aaaGet igång med Python och Azure Cloud Services | Microsoft Docs"
description: "Översikt över användning av Python Tools för Visual Studio toocreate Azure-molntjänster inklusive webb- och arbetsroller."
services: cloud-services
documentationcenter: python
author: thraka
manager: timlt
editor: 
ms.assetid: 5489405d-6fa9-4b11-a161-609103cbdc18
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: f5fd85e754839f146abe912351c59dc4a148c990
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a><span data-ttu-id="542e7-103">Webb- och arbetsroller för Python med Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="542e7-103">Python web and worker roles with Python Tools for Visual Studio</span></span>

<span data-ttu-id="542e7-104">I den här artikeln ges en översikt över hur du använder webb- och arbetsroller för Python med hjälp av [Python Tools för Visual Studio][Python Tools for Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="542e7-104">This article provides an overview of using Python web and worker roles using [Python Tools for Visual Studio][Python Tools for Visual Studio].</span></span> <span data-ttu-id="542e7-105">Lär dig hur toouse Visual Studio toocreate och distribuera en enkel molnbaserad tjänst som använder Python.</span><span class="sxs-lookup"><span data-stu-id="542e7-105">Learn how toouse Visual Studio toocreate and deploy a basic Cloud Service that uses Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="542e7-106">Krav</span><span class="sxs-lookup"><span data-stu-id="542e7-106">Prerequisites</span></span>
* [<span data-ttu-id="542e7-107">Visual Studio 2013, 2015 eller 2017</span><span class="sxs-lookup"><span data-stu-id="542e7-107">Visual Studio 2013, 2015, or 2017</span></span>](https://www.visualstudio.com/)
* <span data-ttu-id="542e7-108">[Python Tools för Visual Studio][Python Tools for Visual Studio] (PTVS)</span><span class="sxs-lookup"><span data-stu-id="542e7-108">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS)</span></span>
* <span data-ttu-id="542e7-109">[Azure SDK Tools för VS 2013][Azure SDK Tools for VS 2013] eller</span><span class="sxs-lookup"><span data-stu-id="542e7-109">[Azure SDK Tools for VS 2013][Azure SDK Tools for VS 2013] or</span></span>  
<span data-ttu-id="542e7-110">[Azure SDK Tools för VS 2015][Azure SDK Tools for VS 2015] eller</span><span class="sxs-lookup"><span data-stu-id="542e7-110">[Azure SDK Tools for VS 2015][Azure SDK Tools for VS 2015] or</span></span>  
<span data-ttu-id="542e7-111">[Azure SDK Tools för VS 2017][Azure SDK Tools for VS 2017]</span><span class="sxs-lookup"><span data-stu-id="542e7-111">[Azure SDK Tools for VS 2017][Azure SDK Tools for VS 2017]</span></span>
* <span data-ttu-id="542e7-112">[Python 2.7 32-bitars][Python 2.7 32-bit] eller [Python 3.5 32-bitars][Python 3.5 32-bit]</span><span class="sxs-lookup"><span data-stu-id="542e7-112">[Python 2.7 32-bit][Python 2.7 32-bit] or [Python 3.5 32-bit][Python 3.5 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a><span data-ttu-id="542e7-113">Vad är webb- och arbetsroller för Python?</span><span class="sxs-lookup"><span data-stu-id="542e7-113">What are Python web and worker roles?</span></span>
<span data-ttu-id="542e7-114">Azure har tre beräkningsmodeller för att köra program: [funktionen Web Apps i Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms] och [Azure Cloud Services][execution model-cloud services].</span><span class="sxs-lookup"><span data-stu-id="542e7-114">Azure provides three compute models for running applications: [Web Apps feature in Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms], and [Azure Cloud Services][execution model-cloud services].</span></span> <span data-ttu-id="542e7-115">Alla tre modeller stöder Python.</span><span class="sxs-lookup"><span data-stu-id="542e7-115">All three models support Python.</span></span> <span data-ttu-id="542e7-116">Cloud Services, där webb- och arbetsroller ingår, tillhandahåller *plattform som en tjänst (PaaS)*.</span><span class="sxs-lookup"><span data-stu-id="542e7-116">Cloud Services, which include web and worker roles, provide *Platform as a Service (PaaS)*.</span></span> <span data-ttu-id="542e7-117">I en molnbaserad tjänst bör tillhandahåller en webbroll en dedikerad Internet Information Services (IIS) web server toohost frontend-webbprogram, medan en arbetsroll kan köra asynkrona, tidskrävande eller beständiga uppgifter oberoende av användarinteraktion eller indata.</span><span class="sxs-lookup"><span data-stu-id="542e7-117">Within a cloud service, a web role provides a dedicated Internet Information Services (IIS) web server toohost front end web applications, while a worker role can run asynchronous, long-running, or perpetual tasks independent of user interaction or input.</span></span>

<span data-ttu-id="542e7-118">Mer information finns i [Vad är en molntjänst?].</span><span class="sxs-lookup"><span data-stu-id="542e7-118">For more information, see [What is a Cloud Service?].</span></span>

> [!NOTE]
> <span data-ttu-id="542e7-119">*Letar du efter toobuild en enkel webbplats?*</span><span class="sxs-lookup"><span data-stu-id="542e7-119">*Looking toobuild a simple website?*</span></span>
> <span data-ttu-id="542e7-120">Om ditt scenario bara en enkel webbplats klientdel, Överväg att använda hello förenklade funktionen Web Apps i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="542e7-120">If your scenario involves just a simple website front end, consider using hello lightweight Web Apps feature in Azure App Service.</span></span> <span data-ttu-id="542e7-121">Du kan lätt att uppgradera tooa Molntjänsten som webbplatsen växer och dina behov förändras.</span><span class="sxs-lookup"><span data-stu-id="542e7-121">You can easily upgrade tooa Cloud Service as your website grows and your requirements change.</span></span> <span data-ttu-id="542e7-122">Se hello <a href="/develop/python/">Python Developer Center</a> för artiklar om utvecklingen av funktionen för hello Web Apps i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="542e7-122">See hello <a href="/develop/python/">Python Developer Center</a> for articles that cover development of hello Web Apps feature in Azure App Service.</span></span>
> <br />
> 
> 

## <a name="project-creation"></a><span data-ttu-id="542e7-123">Skapa projekt</span><span class="sxs-lookup"><span data-stu-id="542e7-123">Project creation</span></span>
<span data-ttu-id="542e7-124">I Visual Studio kan du välja **Azure Cloud Service** i hello **nytt projekt** dialogrutan under **Python**.</span><span class="sxs-lookup"><span data-stu-id="542e7-124">In Visual Studio, you can select **Azure Cloud Service** in hello **New Project** dialog box, under **Python**.</span></span>

![Dialogrutan Nytt projekt](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

<span data-ttu-id="542e7-126">Du kan skapa nya webb- och arbetsroller hello Azure Cloud Service i guiden.</span><span class="sxs-lookup"><span data-stu-id="542e7-126">In hello Azure Cloud Service wizard, you can create new web and worker roles.</span></span>

![Dialogrutan för Azure Cloud Services](./media/cloud-services-python-ptvs/new-service-wizard.png)

<span data-ttu-id="542e7-128">hello mallen för arbetsroller med formaterad kod tooconnect tooan Azure storage-konto eller Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="542e7-128">hello worker role template comes with boilerplate code tooconnect tooan Azure storage account or Azure Service Bus.</span></span>

![Molntjänstlösning](./media/cloud-services-python-ptvs/worker.png)

<span data-ttu-id="542e7-130">Du kan lägga till webb- eller arbetarroll roller tooan befintlig molntjänst när som helst.</span><span class="sxs-lookup"><span data-stu-id="542e7-130">You can add web or worker roles tooan existing cloud service at any time.</span></span>  <span data-ttu-id="542e7-131">Du kan välja tooadd befintliga projekt i din lösning eller skapa nya.</span><span class="sxs-lookup"><span data-stu-id="542e7-131">You can choose tooadd existing projects in your solution, or create new ones.</span></span>

![Kommandot Lägg till roll](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

<span data-ttu-id="542e7-133">Din molntjänst kan innehålla roller som implementeras på olika språk.</span><span class="sxs-lookup"><span data-stu-id="542e7-133">Your cloud service can contain roles implemented in different languages.</span></span>  <span data-ttu-id="542e7-134">Exempelvis kan en Python-webbroll implementeras med hjälp av Django, med Python, eller med C#-arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="542e7-134">For example, you can have a Python web role implemented using Django, with Python, or with C# worker roles.</span></span>  <span data-ttu-id="542e7-135">Du kan enkelt kommunicera mellan dina roller med hjälp av Service Bus-köer eller lagringsköer.</span><span class="sxs-lookup"><span data-stu-id="542e7-135">You can easily communicate between your roles using Service Bus queues or storage queues.</span></span>

## <a name="install-python-on-hello-cloud-service"></a><span data-ttu-id="542e7-136">Installera Python på hello Molntjänsten</span><span class="sxs-lookup"><span data-stu-id="542e7-136">Install Python on hello cloud service</span></span>
> [!WARNING]
> <span data-ttu-id="542e7-137">hello installationsskript som installeras med Visual Studio (för närvarande hello artikeln uppdaterades senast) fungerar inte.</span><span class="sxs-lookup"><span data-stu-id="542e7-137">hello setup scripts that are installed with Visual Studio (at hello time this article was last updated) do not work.</span></span> <span data-ttu-id="542e7-138">Det här avsnittet beskriver en lösning.</span><span class="sxs-lookup"><span data-stu-id="542e7-138">This section describes a workaround.</span></span>
> 
> 

<span data-ttu-id="542e7-139">hello viktigaste problemet med hello installationsskript är att de inte installerar python.</span><span class="sxs-lookup"><span data-stu-id="542e7-139">hello main problem with hello setup scripts is that they do not install python.</span></span> <span data-ttu-id="542e7-140">Först definierar två [Start uppgifter](cloud-services-startup-tasks.md) i hello [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) fil.</span><span class="sxs-lookup"><span data-stu-id="542e7-140">First, define two [startup tasks](cloud-services-startup-tasks.md) in hello [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) file.</span></span> <span data-ttu-id="542e7-141">hello första uppgiften (**PrepPython.ps1**) hämtar och installerar hello Python-körning.</span><span class="sxs-lookup"><span data-stu-id="542e7-141">hello first task (**PrepPython.ps1**) downloads and installs hello Python runtime.</span></span> <span data-ttu-id="542e7-142">hello andra aktiviteten (**PipInstaller.ps1**) och kör pip tooinstall eventuella beroenden som du kan ha.</span><span class="sxs-lookup"><span data-stu-id="542e7-142">hello second task (**PipInstaller.ps1**) runs pip tooinstall any dependencies you may have.</span></span>

<span data-ttu-id="542e7-143">Följande skript hello skrevs riktad Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="542e7-143">hello following scripts were written targeting Python 3.5.</span></span> <span data-ttu-id="542e7-144">Om du vill toouse hello version 2.x av python, ange hello **PYTHON2** variabeln filen för**på** för hello två Start aktiviteter och hello runtime uppgiften: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.</span><span class="sxs-lookup"><span data-stu-id="542e7-144">If you want toouse hello version 2.x of python, set hello **PYTHON2** variable file too**on** for hello two startup tasks and hello runtime task: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.</span></span>

```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>

  </Task>

</Startup>
```

<span data-ttu-id="542e7-145">Hej **PYTHON2** och **PYPATH** toohello worker startaktivitet måste läggas till variabler.</span><span class="sxs-lookup"><span data-stu-id="542e7-145">hello **PYTHON2** and **PYPATH** variables must be added toohello worker startup task.</span></span> <span data-ttu-id="542e7-146">Hej **PYPATH** variabeln används bara om hello **PYTHON2** variabeln anges för**på**.</span><span class="sxs-lookup"><span data-stu-id="542e7-146">hello **PYPATH** variable is only used if hello **PYTHON2** variable is set too**on**.</span></span>

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a><span data-ttu-id="542e7-147">Exempel ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="542e7-147">Sample ServiceDefinition.csdef</span></span>
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



<span data-ttu-id="542e7-148">Skapa sedan hello **PrepPython.ps1** och **PipInstaller.ps1** filer i hello **. / bin** mappen för din roll.</span><span class="sxs-lookup"><span data-stu-id="542e7-148">Next, create hello **PrepPython.ps1** and **PipInstaller.ps1** files in hello **./bin** folder of your role.</span></span>

#### <a name="preppythonps1"></a><span data-ttu-id="542e7-149">PrepPython.ps1</span><span class="sxs-lookup"><span data-stu-id="542e7-149">PrepPython.ps1</span></span>
<span data-ttu-id="542e7-150">Det här skriptet installerar Python.</span><span class="sxs-lookup"><span data-stu-id="542e7-150">This script installs python.</span></span> <span data-ttu-id="542e7-151">Om hello **PYTHON2** miljövariabeln har ställts in för**på**, och sedan Python 2.7 är installerad, annars Python 3.5 är installerat.</span><span class="sxs-lookup"><span data-stu-id="542e7-151">If hello **PYTHON2** environment variable is set too**on**, then Python 2.7 is installed, otherwise Python 3.5 is installed.</span></span>

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }

        Write-Output "Not found, downloading $url too$outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a><span data-ttu-id="542e7-152">PipInstaller.ps1</span><span class="sxs-lookup"><span data-stu-id="542e7-152">PipInstaller.ps1</span></span>
<span data-ttu-id="542e7-153">Det här skriptet ringer upp pip och installerar alla hello beroenden i hello **requirements.txt** fil.</span><span class="sxs-lookup"><span data-stu-id="542e7-153">This script calls up pip and installs all of hello dependencies in hello **requirements.txt** file.</span></span> <span data-ttu-id="542e7-154">Om hello **PYTHON2** miljövariabeln har ställts in för**på**, Python 2.7 används, annars Python 3.5 används.</span><span class="sxs-lookup"><span data-stu-id="542e7-154">If hello **PYTHON2** environment variable is set too**on**, then Python 2.7 is used, otherwise Python 3.5 is used.</span></span>

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a><span data-ttu-id="542e7-155">Ändra LaunchWorker.ps1</span><span class="sxs-lookup"><span data-stu-id="542e7-155">Modify LaunchWorker.ps1</span></span>
> [!NOTE]
> <span data-ttu-id="542e7-156">Hello gäller en **arbetsrollen** projektet **LauncherWorker.ps1** är obligatoriska tooexecute hello Start-fil.</span><span class="sxs-lookup"><span data-stu-id="542e7-156">In hello case of a **worker role** project, **LauncherWorker.ps1** file is required tooexecute hello startup file.</span></span> <span data-ttu-id="542e7-157">I en **webbroll** projekt, hello startfil i stället definieras i hello projektegenskaperna.</span><span class="sxs-lookup"><span data-stu-id="542e7-157">In a **web role** project, hello startup file is instead defined in hello project properties.</span></span>
> 
> 

<span data-ttu-id="542e7-158">Hej **bin\LaunchWorker.ps1** ursprungligen skapades toodo mycket prep arbete, men det inte fungerar.</span><span class="sxs-lookup"><span data-stu-id="542e7-158">hello **bin\LaunchWorker.ps1** was originally created toodo a lot of prep work but it doesn't really work.</span></span> <span data-ttu-id="542e7-159">Ersätt hello innehållet i filen med hello följande skript.</span><span class="sxs-lookup"><span data-stu-id="542e7-159">Replace hello contents in that file with hello following script.</span></span>

<span data-ttu-id="542e7-160">Det här skriptet anropar hello **worker.py** filen från projektet python.</span><span class="sxs-lookup"><span data-stu-id="542e7-160">This script calls hello **worker.py** file from your python project.</span></span> <span data-ttu-id="542e7-161">Om hello **PYTHON2** miljövariabeln har ställts in för**på**, Python 2.7 används, annars Python 3.5 används.</span><span class="sxs-lookup"><span data-stu-id="542e7-161">If hello **PYTHON2** environment variable is set too**on**, then Python 2.7 is used, otherwise Python 3.5 is used.</span></span>

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize tooyour local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a><span data-ttu-id="542e7-162">ps.cmd</span><span class="sxs-lookup"><span data-stu-id="542e7-162">ps.cmd</span></span>
<span data-ttu-id="542e7-163">hello Visual Studio mallar ha skapat en **ps.cmd** filen i hello **. / bin** mapp.</span><span class="sxs-lookup"><span data-stu-id="542e7-163">hello Visual Studio templates should have created a **ps.cmd** file in hello **./bin** folder.</span></span> <span data-ttu-id="542e7-164">Shell-skript visar hello PowerShell wrapper skript ovan och tillhandahåller loggning baserat på hello namn för hello PowerShell wrapper anropas.</span><span class="sxs-lookup"><span data-stu-id="542e7-164">This shell script calls out hello PowerShell wrapper scripts above and provides logging based on hello name of hello PowerShell wrapper called.</span></span> <span data-ttu-id="542e7-165">Detta är vad som bör finnas i den, om filen inte skapades.</span><span class="sxs-lookup"><span data-stu-id="542e7-165">If this file wasn't created, here is what should be in it.</span></span> 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a><span data-ttu-id="542e7-166">Lokal körning</span><span class="sxs-lookup"><span data-stu-id="542e7-166">Run locally</span></span>
<span data-ttu-id="542e7-167">Om du konfigurera ditt molntjänstprojekt som hello Startprojekt och tryck på F5 körs Molntjänsten hello i hello lokala Azure-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="542e7-167">If you set your cloud service project as hello startup project and press F5, hello cloud service runs in hello local Azure emulator.</span></span>

<span data-ttu-id="542e7-168">Även om PTVS stöder fungerar startas i hello emulatorn felsökning (till exempel brytpunkter) inte.</span><span class="sxs-lookup"><span data-stu-id="542e7-168">Although PTVS supports launching in hello emulator, debugging (for example, breakpoints) does not work.</span></span>

<span data-ttu-id="542e7-169">toodebug dina webb- och arbetsroller roller som du kan ange hello rollen projektet som Startprojekt hello och felsöka det i stället.</span><span class="sxs-lookup"><span data-stu-id="542e7-169">toodebug your web and worker roles, you can set hello role project as hello startup project and debug that instead.</span></span>  <span data-ttu-id="542e7-170">Du kan också ange flera startprojekt.</span><span class="sxs-lookup"><span data-stu-id="542e7-170">You can also set multiple startup projects.</span></span>  <span data-ttu-id="542e7-171">Högerklicka på hello lösning och välj sedan **ange Startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="542e7-171">Right-click hello solution and then select **Set StartUp Projects**.</span></span>

![Egenskaper för lösningens startprojekt](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-tooazure"></a><span data-ttu-id="542e7-173">Publicera tooAzure</span><span class="sxs-lookup"><span data-stu-id="542e7-173">Publish tooAzure</span></span>
<span data-ttu-id="542e7-174">toopublish, högerklicka på hello molntjänstprojektet i lösningen hello och välj sedan **publicera**.</span><span class="sxs-lookup"><span data-stu-id="542e7-174">toopublish, right-click hello cloud service project in hello solution and then select **Publish**.</span></span>

![Inloggning för publicering i Microsoft Azure](./media/cloud-services-python-ptvs/publish-sign-in.png)

<span data-ttu-id="542e7-176">Följ guiden hello.</span><span class="sxs-lookup"><span data-stu-id="542e7-176">Follow hello wizard.</span></span> <span data-ttu-id="542e7-177">Aktivera Fjärrskrivbord vid behov.</span><span class="sxs-lookup"><span data-stu-id="542e7-177">If you need to, enable remote desktop.</span></span> <span data-ttu-id="542e7-178">Fjärrskrivbord är användbart när du behöver toodebug något.</span><span class="sxs-lookup"><span data-stu-id="542e7-178">Remote desktop is helpful when you need toodebug something.</span></span>

<span data-ttu-id="542e7-179">När du har konfigurerat inställningarna klickar du på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="542e7-179">When you are done configuring settings, click **Publish**.</span></span>

<span data-ttu-id="542e7-180">Förloppet visas i utdatafönstret hello och sedan visas hello Azure-aktivitetsloggen fönster.</span><span class="sxs-lookup"><span data-stu-id="542e7-180">Some progress appears in hello output window, then you'll see hello Microsoft Azure Activity Log window.</span></span>

![Fönstret med Microsoft Azure-aktivitetsloggen](./media/cloud-services-python-ptvs/publish-activity-log.png)

<span data-ttu-id="542e7-182">Distributionen tar flera minuter toocomplete, sedan webbplatsen och/eller arbetsroller köras i Azure!</span><span class="sxs-lookup"><span data-stu-id="542e7-182">Deployment takes several minutes toocomplete, then your web and/or worker roles run on Azure!</span></span>

### <a name="investigate-logs"></a><span data-ttu-id="542e7-183">Undersöka loggar</span><span class="sxs-lookup"><span data-stu-id="542e7-183">Investigate logs</span></span>
<span data-ttu-id="542e7-184">När hello cloud service virtuell dator startar och installerar Python, kan du titta på hello loggar toofind eventuella felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="542e7-184">After hello cloud service virtual machine starts up and installs Python, you can look at hello logs toofind any failure messages.</span></span> <span data-ttu-id="542e7-185">Dessa loggar finns i hello **C:\Resources\Directory\\{rollen} \LogFiles** mapp.</span><span class="sxs-lookup"><span data-stu-id="542e7-185">These logs are located in hello **C:\Resources\Directory\\{role}\LogFiles** folder.</span></span> <span data-ttu-id="542e7-186">**PrepPython.err.txt** har minst ett fel i den från när hello-skriptet försöker toodetect om Python är installerad och **PipInstaller.err.txt** kan klagar på att en inaktuell version av pip.</span><span class="sxs-lookup"><span data-stu-id="542e7-186">**PrepPython.err.txt** has at least one error in it from when hello script tries toodetect if Python is installed and **PipInstaller.err.txt** may complain about an outdated version of pip.</span></span>

## <a name="next-steps"></a><span data-ttu-id="542e7-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="542e7-187">Next steps</span></span>
<span data-ttu-id="542e7-188">Mer detaljerad information om hur du arbetar med webb- och arbetsroller roller i Python Tools för Visual Studio finns i dokumentationen till PTVS hello:</span><span class="sxs-lookup"><span data-stu-id="542e7-188">For more detailed information about working with web and worker roles in Python Tools for Visual Studio, see hello PTVS documentation:</span></span>

* <span data-ttu-id="542e7-189">[Molntjänstprojekt][Cloud Service Projects]</span><span class="sxs-lookup"><span data-stu-id="542e7-189">[Cloud Service Projects][Cloud Service Projects]</span></span>

<span data-ttu-id="542e7-190">Mer information om hur du använder Azure-tjänster från dina webb- och arbetsroller roller, till exempel med hjälp av Azure Storage eller Azure Service Bus finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="542e7-190">For more details about using Azure services from your web and worker roles, such as using Azure Storage or Service Bus, see hello following articles:</span></span>

* <span data-ttu-id="542e7-191">[Blob Service][Blob Service]</span><span class="sxs-lookup"><span data-stu-id="542e7-191">[Blob Service][Blob Service]</span></span>
* <span data-ttu-id="542e7-192">[Table Service][Table Service]</span><span class="sxs-lookup"><span data-stu-id="542e7-192">[Table Service][Table Service]</span></span>
* <span data-ttu-id="542e7-193">[Kötjänst][Queue Service]</span><span class="sxs-lookup"><span data-stu-id="542e7-193">[Queue Service][Queue Service]</span></span>
* <span data-ttu-id="542e7-194">[Service Bus-köer][Service Bus Queues]</span><span class="sxs-lookup"><span data-stu-id="542e7-194">[Service Bus Queues][Service Bus Queues]</span></span>
* <span data-ttu-id="542e7-195">[Service Bus-avsnitt][Service Bus Topics]</span><span class="sxs-lookup"><span data-stu-id="542e7-195">[Service Bus Topics][Service Bus Topics]</span></span>

<!--Link references-->

[Vad är en molntjänst?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]:../virtual-machines/windows/overview.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[Blob Service]:../storage/blobs/storage-python-how-to-use-blob-storage.md
[Queue Service]: ../storage/queues/storage-python-how-to-use-queue-storage.md
[Table Service]:../cosmos-db/table-storage-how-to-use-python.md
[Service Bus Queues]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Service Bus Topics]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Cloud Service Projects]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=746482
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=746481
[Azure SDK Tools for VS 2017]: http://go.microsoft.com/fwlink/?LinkId=746483
[Python 2.7 32-bit]: https://www.python.org/downloads/
[Python 3.5 32-bit]: https://www.python.org/downloads/
