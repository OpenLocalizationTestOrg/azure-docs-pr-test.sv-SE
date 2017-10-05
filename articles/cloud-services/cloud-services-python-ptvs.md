---
title: "Komma igång med Python och Azure Cloud Services | Microsoft Docs"
description: "Översikt över hur du använder Python Tools för Visual Studio för att skapa Azure-molntjänster, inklusive webb- och arbetsroller."
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
ms.openlocfilehash: 7d2bc89943087323e92cf06981bbacaf4b8ff060
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a><span data-ttu-id="e5193-103">Webb- och arbetsroller för Python med Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e5193-103">Python web and worker roles with Python Tools for Visual Studio</span></span>

<span data-ttu-id="e5193-104">I den här artikeln ges en översikt över hur du använder webb- och arbetsroller för Python med hjälp av [Python Tools för Visual Studio][Python Tools for Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="e5193-104">This article provides an overview of using Python web and worker roles using [Python Tools for Visual Studio][Python Tools for Visual Studio].</span></span> <span data-ttu-id="e5193-105">Lär dig hur du använder Visual Studio för att skapa och distribuera en grundläggande molntjänst som använder Python.</span><span class="sxs-lookup"><span data-stu-id="e5193-105">Learn how to use Visual Studio to create and deploy a basic Cloud Service that uses Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5193-106">Krav</span><span class="sxs-lookup"><span data-stu-id="e5193-106">Prerequisites</span></span>
* [<span data-ttu-id="e5193-107">Visual Studio 2013, 2015 eller 2017</span><span class="sxs-lookup"><span data-stu-id="e5193-107">Visual Studio 2013, 2015, or 2017</span></span>](https://www.visualstudio.com/)
* <span data-ttu-id="e5193-108">[Python Tools för Visual Studio][Python Tools for Visual Studio] (PTVS)</span><span class="sxs-lookup"><span data-stu-id="e5193-108">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS)</span></span>
* <span data-ttu-id="e5193-109">[Azure SDK Tools för VS 2013][Azure SDK Tools for VS 2013] eller</span><span class="sxs-lookup"><span data-stu-id="e5193-109">[Azure SDK Tools for VS 2013][Azure SDK Tools for VS 2013] or</span></span>  
<span data-ttu-id="e5193-110">[Azure SDK Tools för VS 2015][Azure SDK Tools for VS 2015] eller</span><span class="sxs-lookup"><span data-stu-id="e5193-110">[Azure SDK Tools for VS 2015][Azure SDK Tools for VS 2015] or</span></span>  
<span data-ttu-id="e5193-111">[Azure SDK Tools för VS 2017][Azure SDK Tools for VS 2017]</span><span class="sxs-lookup"><span data-stu-id="e5193-111">[Azure SDK Tools for VS 2017][Azure SDK Tools for VS 2017]</span></span>
* <span data-ttu-id="e5193-112">[Python 2.7 32-bitars][Python 2.7 32-bit] eller [Python 3.5 32-bitars][Python 3.5 32-bit]</span><span class="sxs-lookup"><span data-stu-id="e5193-112">[Python 2.7 32-bit][Python 2.7 32-bit] or [Python 3.5 32-bit][Python 3.5 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a><span data-ttu-id="e5193-113">Vad är webb- och arbetsroller för Python?</span><span class="sxs-lookup"><span data-stu-id="e5193-113">What are Python web and worker roles?</span></span>
<span data-ttu-id="e5193-114">Azure har tre beräkningsmodeller för att köra program: [funktionen Web Apps i Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms] och [Azure Cloud Services][execution model-cloud services].</span><span class="sxs-lookup"><span data-stu-id="e5193-114">Azure provides three compute models for running applications: [Web Apps feature in Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms], and [Azure Cloud Services][execution model-cloud services].</span></span> <span data-ttu-id="e5193-115">Alla tre modeller stöder Python.</span><span class="sxs-lookup"><span data-stu-id="e5193-115">All three models support Python.</span></span> <span data-ttu-id="e5193-116">Cloud Services, där webb- och arbetsroller ingår, tillhandahåller *plattform som en tjänst (PaaS)*.</span><span class="sxs-lookup"><span data-stu-id="e5193-116">Cloud Services, which include web and worker roles, provide *Platform as a Service (PaaS)*.</span></span> <span data-ttu-id="e5193-117">I en molntjänst tillhandahåller en webbroll en dedikerad IIS-webbserver (Internet Information Services) som fungerar som värd för frontend-webbprogram, medan en arbetsroll kan köra asynkrona, tidskrävande eller beständiga uppgifter oberoende av användarinteraktion eller indata.</span><span class="sxs-lookup"><span data-stu-id="e5193-117">Within a cloud service, a web role provides a dedicated Internet Information Services (IIS) web server to host front end web applications, while a worker role can run asynchronous, long-running, or perpetual tasks independent of user interaction or input.</span></span>

<span data-ttu-id="e5193-118">Mer information finns i [Vad är en molntjänst?].</span><span class="sxs-lookup"><span data-stu-id="e5193-118">For more information, see [What is a Cloud Service?].</span></span>

> [!NOTE]
> <span data-ttu-id="e5193-119">*Vill du skapa en enkel webbplats?*</span><span class="sxs-lookup"><span data-stu-id="e5193-119">*Looking to build a simple website?*</span></span>
> <span data-ttu-id="e5193-120">Om ditt scenario bara är en enkel webbplats bör du överväga att använda den förenklade funktionen Web Apps i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e5193-120">If your scenario involves just a simple website front end, consider using the lightweight Web Apps feature in Azure App Service.</span></span> <span data-ttu-id="e5193-121">Det är lätt att uppgradera till en molntjänst efter hand som webbplatsen växer och dina behov förändras.</span><span class="sxs-lookup"><span data-stu-id="e5193-121">You can easily upgrade to a Cloud Service as your website grows and your requirements change.</span></span> <span data-ttu-id="e5193-122">På <a href="/develop/python/">Python Developer Center</a> hittar du artiklar om utvecklingen av funktionen Web Apps i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e5193-122">See the <a href="/develop/python/">Python Developer Center</a> for articles that cover development of the Web Apps feature in Azure App Service.</span></span>
> <br />
> 
> 

## <a name="project-creation"></a><span data-ttu-id="e5193-123">Skapa projekt</span><span class="sxs-lookup"><span data-stu-id="e5193-123">Project creation</span></span>
<span data-ttu-id="e5193-124">I Visual Studio kan du välja **Azure Cloud Service** i dialogrutan **Nytt projekt** under **Python**.</span><span class="sxs-lookup"><span data-stu-id="e5193-124">In Visual Studio, you can select **Azure Cloud Service** in the **New Project** dialog box, under **Python**.</span></span>

![Dialogrutan Nytt projekt](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

<span data-ttu-id="e5193-126">Du kan skapa nya webb- och arbetsroller i Azure Cloud Services-guiden.</span><span class="sxs-lookup"><span data-stu-id="e5193-126">In the Azure Cloud Service wizard, you can create new web and worker roles.</span></span>

![Dialogrutan för Azure Cloud Services](./media/cloud-services-python-ptvs/new-service-wizard.png)

<span data-ttu-id="e5193-128">Mallen för arbetsroller innehåller formaterad exempelkod för anslutning till ett Azure-lagringskonto eller till Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="e5193-128">The worker role template comes with boilerplate code to connect to an Azure storage account or Azure Service Bus.</span></span>

![Molntjänstlösning](./media/cloud-services-python-ptvs/worker.png)

<span data-ttu-id="e5193-130">Du kan lägga till webb- eller arbetsroller till en befintlig molntjänst när som helst.</span><span class="sxs-lookup"><span data-stu-id="e5193-130">You can add web or worker roles to an existing cloud service at any time.</span></span>  <span data-ttu-id="e5193-131">Du kan välja att lägga till befintliga projekt i din lösning eller skapa nya.</span><span class="sxs-lookup"><span data-stu-id="e5193-131">You can choose to add existing projects in your solution, or create new ones.</span></span>

![Kommandot Lägg till roll](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

<span data-ttu-id="e5193-133">Din molntjänst kan innehålla roller som implementeras på olika språk.</span><span class="sxs-lookup"><span data-stu-id="e5193-133">Your cloud service can contain roles implemented in different languages.</span></span>  <span data-ttu-id="e5193-134">Exempelvis kan en Python-webbroll implementeras med hjälp av Django, med Python, eller med C#-arbetsroller.</span><span class="sxs-lookup"><span data-stu-id="e5193-134">For example, you can have a Python web role implemented using Django, with Python, or with C# worker roles.</span></span>  <span data-ttu-id="e5193-135">Du kan enkelt kommunicera mellan dina roller med hjälp av Service Bus-köer eller lagringsköer.</span><span class="sxs-lookup"><span data-stu-id="e5193-135">You can easily communicate between your roles using Service Bus queues or storage queues.</span></span>

## <a name="install-python-on-the-cloud-service"></a><span data-ttu-id="e5193-136">Installera Python i molntjänsten</span><span class="sxs-lookup"><span data-stu-id="e5193-136">Install Python on the cloud service</span></span>
> [!WARNING]
> <span data-ttu-id="e5193-137">Installationsskripten som installeras med Visual Studio (när den här artikeln uppdaterades senast) fungerar inte.</span><span class="sxs-lookup"><span data-stu-id="e5193-137">The setup scripts that are installed with Visual Studio (at the time this article was last updated) do not work.</span></span> <span data-ttu-id="e5193-138">Det här avsnittet beskriver en lösning.</span><span class="sxs-lookup"><span data-stu-id="e5193-138">This section describes a workaround.</span></span>
> 
> 

<span data-ttu-id="e5193-139">Huvudproblemet med installationsskripten är att de inte installerar Python.</span><span class="sxs-lookup"><span data-stu-id="e5193-139">The main problem with the setup scripts is that they do not install python.</span></span> <span data-ttu-id="e5193-140">Definiera först två [startaktiviteter](cloud-services-startup-tasks.md) i filen [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef).</span><span class="sxs-lookup"><span data-stu-id="e5193-140">First, define two [startup tasks](cloud-services-startup-tasks.md) in the [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) file.</span></span> <span data-ttu-id="e5193-141">Den första aktiviteten (**PrepPython.ps1**) hämtar och installerar Python-körningen.</span><span class="sxs-lookup"><span data-stu-id="e5193-141">The first task (**PrepPython.ps1**) downloads and installs the Python runtime.</span></span> <span data-ttu-id="e5193-142">Den andra aktiviteten (**PipInstaller.ps1**) kör pip för att installera alla beroenden som du kan ha.</span><span class="sxs-lookup"><span data-stu-id="e5193-142">The second task (**PipInstaller.ps1**) runs pip to install any dependencies you may have.</span></span>

<span data-ttu-id="e5193-143">Följande skript har skrivits för Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="e5193-143">The following scripts were written targeting Python 3.5.</span></span> <span data-ttu-id="e5193-144">Om du vill använda version 2.x av Python, ställer du in **PYTHON2**-variabelfilen till **på** för de båda startaktiviteterna och för körningsaktiviteten: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.</span><span class="sxs-lookup"><span data-stu-id="e5193-144">If you want to use the version 2.x of python, set the **PYTHON2** variable file to **on** for the two startup tasks and the runtime task: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.</span></span>

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

<span data-ttu-id="e5193-145">Variablerna **PYTHON2** och **PYPATH** måste läggas till arbetsstartsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="e5193-145">The **PYTHON2** and **PYPATH** variables must be added to the worker startup task.</span></span> <span data-ttu-id="e5193-146">**PYPATH**-variabeln används bara om **PYTHON2**-variabeln anges till **på**.</span><span class="sxs-lookup"><span data-stu-id="e5193-146">The **PYPATH** variable is only used if the **PYTHON2** variable is set to **on**.</span></span>

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

#### <a name="sample-servicedefinitioncsdef"></a><span data-ttu-id="e5193-147">Exempel ServiceDefinition.csdef</span><span class="sxs-lookup"><span data-stu-id="e5193-147">Sample ServiceDefinition.csdef</span></span>
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



<span data-ttu-id="e5193-148">Skapa sedan filerna **PrepPython.ps1** och **PipInstaller.ps1** i din rolls **. / bin**-mapp.</span><span class="sxs-lookup"><span data-stu-id="e5193-148">Next, create the **PrepPython.ps1** and **PipInstaller.ps1** files in the **./bin** folder of your role.</span></span>

#### <a name="preppythonps1"></a><span data-ttu-id="e5193-149">PrepPython.ps1</span><span class="sxs-lookup"><span data-stu-id="e5193-149">PrepPython.ps1</span></span>
<span data-ttu-id="e5193-150">Det här skriptet installerar Python.</span><span class="sxs-lookup"><span data-stu-id="e5193-150">This script installs python.</span></span> <span data-ttu-id="e5193-151">Om du anger **PYTHON2**-miljövariabeln till **på** installeras Python 2.7, annars installeras Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="e5193-151">If the **PYTHON2** environment variable is set to **on**, then Python 2.7 is installed, otherwise Python 3.5 is installed.</span></span>

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

        Write-Output "Not found, downloading $url to $outFile$nl"
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

#### <a name="pipinstallerps1"></a><span data-ttu-id="e5193-152">PipInstaller.ps1</span><span class="sxs-lookup"><span data-stu-id="e5193-152">PipInstaller.ps1</span></span>
<span data-ttu-id="e5193-153">Det här skriptet ringer upp pip och installerar alla beroenden i **requirements.txt**-filen.</span><span class="sxs-lookup"><span data-stu-id="e5193-153">This script calls up pip and installs all of the dependencies in the **requirements.txt** file.</span></span> <span data-ttu-id="e5193-154">Om du anger **PYTHON2**-miljövariabeln till **på** används Python 2.7, annars används Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="e5193-154">If the **PYTHON2** environment variable is set to **on**, then Python 2.7 is used, otherwise Python 3.5 is used.</span></span>

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

#### <a name="modify-launchworkerps1"></a><span data-ttu-id="e5193-155">Ändra LaunchWorker.ps1</span><span class="sxs-lookup"><span data-stu-id="e5193-155">Modify LaunchWorker.ps1</span></span>
> [!NOTE]
> <span data-ttu-id="e5193-156">Vid ett **arbetsroll**projekt krävs filen **LauncherWorker.ps1** för att köra startfilen.</span><span class="sxs-lookup"><span data-stu-id="e5193-156">In the case of a **worker role** project, **LauncherWorker.ps1** file is required to execute the startup file.</span></span> <span data-ttu-id="e5193-157">Vid ett **webbroll**projekt definieras startfilen istället i egenskaperna för projektet.</span><span class="sxs-lookup"><span data-stu-id="e5193-157">In a **web role** project, the startup file is instead defined in the project properties.</span></span>
> 
> 

<span data-ttu-id="e5193-158">**bin\LaunchWorker.ps1** skapades ursprungligen för att göra mycket förberedande arbete men det fungerar inte riktigt som tänkt.</span><span class="sxs-lookup"><span data-stu-id="e5193-158">The **bin\LaunchWorker.ps1** was originally created to do a lot of prep work but it doesn't really work.</span></span> <span data-ttu-id="e5193-159">Ersätt innehållet i filen med följande skript.</span><span class="sxs-lookup"><span data-stu-id="e5193-159">Replace the contents in that file with the following script.</span></span>

<span data-ttu-id="e5193-160">Det här skriptet anropar **worker.py**-filen från python-projektet.</span><span class="sxs-lookup"><span data-stu-id="e5193-160">This script calls the **worker.py** file from your python project.</span></span> <span data-ttu-id="e5193-161">Om du anger **PYTHON2**-miljövariabeln till **på** används Python 2.7, annars används Python 3.5.</span><span class="sxs-lookup"><span data-stu-id="e5193-161">If the **PYTHON2** environment variable is set to **on**, then Python 2.7 is used, otherwise Python 3.5 is used.</span></span>

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

    # Customize to your local dev environment

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

#### <a name="pscmd"></a><span data-ttu-id="e5193-162">ps.cmd</span><span class="sxs-lookup"><span data-stu-id="e5193-162">ps.cmd</span></span>
<span data-ttu-id="e5193-163">Visual Studio-mallarna ska ha skapat en **ps.cmd**-fil i **. / bin**-mappen.</span><span class="sxs-lookup"><span data-stu-id="e5193-163">The Visual Studio templates should have created a **ps.cmd** file in the **./bin** folder.</span></span> <span data-ttu-id="e5193-164">Detta gränssnittsskript anropar PowerShell-omslutningsskripten ovan och tillhandahåller loggning baserat på namnet på den anropade PowerShell-omslutningen.</span><span class="sxs-lookup"><span data-stu-id="e5193-164">This shell script calls out the PowerShell wrapper scripts above and provides logging based on the name of the PowerShell wrapper called.</span></span> <span data-ttu-id="e5193-165">Detta är vad som bör finnas i den, om filen inte skapades.</span><span class="sxs-lookup"><span data-stu-id="e5193-165">If this file wasn't created, here is what should be in it.</span></span> 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a><span data-ttu-id="e5193-166">Lokal körning</span><span class="sxs-lookup"><span data-stu-id="e5193-166">Run locally</span></span>
<span data-ttu-id="e5193-167">Om du definierar ditt molntjänstprojekt som startprojektet och trycker på F5 körs molntjänsten i den lokala Azure-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="e5193-167">If you set your cloud service project as the startup project and press F5, the cloud service runs in the local Azure emulator.</span></span>

<span data-ttu-id="e5193-168">Även om PTVS har stöd för att starta i emulatorn så går det inte att felsöka (till exempel brytpunkter).</span><span class="sxs-lookup"><span data-stu-id="e5193-168">Although PTVS supports launching in the emulator, debugging (for example, breakpoints) does not work.</span></span>

<span data-ttu-id="e5193-169">Om du vill felsöka dina webb- och arbetsroller kan du konfigurera rollprojektet som startprojektet och felsöka det i stället.</span><span class="sxs-lookup"><span data-stu-id="e5193-169">To debug your web and worker roles, you can set the role project as the startup project and debug that instead.</span></span>  <span data-ttu-id="e5193-170">Du kan också ange flera startprojekt.</span><span class="sxs-lookup"><span data-stu-id="e5193-170">You can also set multiple startup projects.</span></span>  <span data-ttu-id="e5193-171">Högerklicka på lösningen och välj **Ange startprojekt**.</span><span class="sxs-lookup"><span data-stu-id="e5193-171">Right-click the solution and then select **Set StartUp Projects**.</span></span>

![Egenskaper för lösningens startprojekt](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a><span data-ttu-id="e5193-173">Publicera till Azure</span><span class="sxs-lookup"><span data-stu-id="e5193-173">Publish to Azure</span></span>
<span data-ttu-id="e5193-174">När du vill publicera högerklickar du på molntjänstprojektet i lösningen och väljer **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="e5193-174">To publish, right-click the cloud service project in the solution and then select **Publish**.</span></span>

![Inloggning för publicering i Microsoft Azure](./media/cloud-services-python-ptvs/publish-sign-in.png)

<span data-ttu-id="e5193-176">Följ guiden.</span><span class="sxs-lookup"><span data-stu-id="e5193-176">Follow the wizard.</span></span> <span data-ttu-id="e5193-177">Aktivera Fjärrskrivbord vid behov.</span><span class="sxs-lookup"><span data-stu-id="e5193-177">If you need to, enable remote desktop.</span></span> <span data-ttu-id="e5193-178">Fjärrskrivbord är användbart när du behöver felsöka något.</span><span class="sxs-lookup"><span data-stu-id="e5193-178">Remote desktop is helpful when you need to debug something.</span></span>

<span data-ttu-id="e5193-179">När du har konfigurerat inställningarna klickar du på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="e5193-179">When you are done configuring settings, click **Publish**.</span></span>

<span data-ttu-id="e5193-180">Förloppet visas i utdatafönstret. Därefter visas fönstret med Microsoft Azure-aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="e5193-180">Some progress appears in the output window, then you'll see the Microsoft Azure Activity Log window.</span></span>

![Fönstret med Microsoft Azure-aktivitetsloggen](./media/cloud-services-python-ptvs/publish-activity-log.png)

<span data-ttu-id="e5193-182">Distributionen tar flera minuter. När den är klar körs dina webb- och/eller arbetsroller i Azure!</span><span class="sxs-lookup"><span data-stu-id="e5193-182">Deployment takes several minutes to complete, then your web and/or worker roles run on Azure!</span></span>

### <a name="investigate-logs"></a><span data-ttu-id="e5193-183">Undersöka loggar</span><span class="sxs-lookup"><span data-stu-id="e5193-183">Investigate logs</span></span>
<span data-ttu-id="e5193-184">När den virtuella molntjänstdatorn startar och installerar Python, kan du titta på loggarna för att hitta eventuella felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="e5193-184">After the cloud service virtual machine starts up and installs Python, you can look at the logs to find any failure messages.</span></span> <span data-ttu-id="e5193-185">Dessa loggar finns i mappen **C:\Resources\Directory\\{role}\LogFiles**.</span><span class="sxs-lookup"><span data-stu-id="e5193-185">These logs are located in the **C:\Resources\Directory\\{role}\LogFiles** folder.</span></span> <span data-ttu-id="e5193-186">**PrepPython.err.txt** har minst ett fel från när skriptet försöker identifiera om Python är installerat och **PipInstaller.err.txt** kan klaga över en inaktuell version av pip.</span><span class="sxs-lookup"><span data-stu-id="e5193-186">**PrepPython.err.txt** has at least one error in it from when the script tries to detect if Python is installed and **PipInstaller.err.txt** may complain about an outdated version of pip.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5193-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5193-187">Next steps</span></span>
<span data-ttu-id="e5193-188">Mer detaljerad information om hur du arbetar med webb- och arbetsroller i Python Tools för Visual Studio finns i dokumentationen till PTVS:</span><span class="sxs-lookup"><span data-stu-id="e5193-188">For more detailed information about working with web and worker roles in Python Tools for Visual Studio, see the PTVS documentation:</span></span>

* <span data-ttu-id="e5193-189">[Molntjänstprojekt][Cloud Service Projects]</span><span class="sxs-lookup"><span data-stu-id="e5193-189">[Cloud Service Projects][Cloud Service Projects]</span></span>

<span data-ttu-id="e5193-190">Mer information om hur du använder Azure-tjänster från dina webb- och arbetsroller, t.ex. Azure Storage eller Azure Service Bus, finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="e5193-190">For more details about using Azure services from your web and worker roles, such as using Azure Storage or Service Bus, see the following articles:</span></span>

* <span data-ttu-id="e5193-191">[Blob Service][Blob Service]</span><span class="sxs-lookup"><span data-stu-id="e5193-191">[Blob Service][Blob Service]</span></span>
* <span data-ttu-id="e5193-192">[Table Service][Table Service]</span><span class="sxs-lookup"><span data-stu-id="e5193-192">[Table Service][Table Service]</span></span>
* <span data-ttu-id="e5193-193">[Kötjänst][Queue Service]</span><span class="sxs-lookup"><span data-stu-id="e5193-193">[Queue Service][Queue Service]</span></span>
* <span data-ttu-id="e5193-194">[Service Bus-köer][Service Bus Queues]</span><span class="sxs-lookup"><span data-stu-id="e5193-194">[Service Bus Queues][Service Bus Queues]</span></span>
* <span data-ttu-id="e5193-195">[Service Bus-avsnitt][Service Bus Topics]</span><span class="sxs-lookup"><span data-stu-id="e5193-195">[Service Bus Topics][Service Bus Topics]</span></span>

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
