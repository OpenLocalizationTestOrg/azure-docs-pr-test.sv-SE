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
# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Webb- och arbetsroller för Python med Python Tools för Visual Studio

I den här artikeln ges en översikt över hur du använder webb- och arbetsroller för Python med hjälp av [Python Tools för Visual Studio][Python Tools for Visual Studio]. Lär dig hur toouse Visual Studio toocreate och distribuera en enkel molnbaserad tjänst som använder Python.

## <a name="prerequisites"></a>Krav
* [Visual Studio 2013, 2015 eller 2017](https://www.visualstudio.com/)
* [Python Tools för Visual Studio][Python Tools for Visual Studio] (PTVS)
* [Azure SDK Tools för VS 2013][Azure SDK Tools for VS 2013] eller  
[Azure SDK Tools för VS 2015][Azure SDK Tools for VS 2015] eller  
[Azure SDK Tools för VS 2017][Azure SDK Tools for VS 2017]
* [Python 2.7 32-bitars][Python 2.7 32-bit] eller [Python 3.5 32-bitars][Python 3.5 32-bit]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Vad är webb- och arbetsroller för Python?
Azure har tre beräkningsmodeller för att köra program: [funktionen Web Apps i Azure App Service][execution model-web sites], [Azure Virtual Machines][execution model-vms] och [Azure Cloud Services][execution model-cloud services]. Alla tre modeller stöder Python. Cloud Services, där webb- och arbetsroller ingår, tillhandahåller *plattform som en tjänst (PaaS)*. I en molnbaserad tjänst bör tillhandahåller en webbroll en dedikerad Internet Information Services (IIS) web server toohost frontend-webbprogram, medan en arbetsroll kan köra asynkrona, tidskrävande eller beständiga uppgifter oberoende av användarinteraktion eller indata.

Mer information finns i [Vad är en molntjänst?].

> [!NOTE]
> *Letar du efter toobuild en enkel webbplats?*
> Om ditt scenario bara en enkel webbplats klientdel, Överväg att använda hello förenklade funktionen Web Apps i Azure App Service. Du kan lätt att uppgradera tooa Molntjänsten som webbplatsen växer och dina behov förändras. Se hello <a href="/develop/python/">Python Developer Center</a> för artiklar om utvecklingen av funktionen för hello Web Apps i Azure App Service.
> <br />
> 
> 

## <a name="project-creation"></a>Skapa projekt
I Visual Studio kan du välja **Azure Cloud Service** i hello **nytt projekt** dialogrutan under **Python**.

![Dialogrutan Nytt projekt](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Du kan skapa nya webb- och arbetsroller hello Azure Cloud Service i guiden.

![Dialogrutan för Azure Cloud Services](./media/cloud-services-python-ptvs/new-service-wizard.png)

hello mallen för arbetsroller med formaterad kod tooconnect tooan Azure storage-konto eller Azure Service Bus.

![Molntjänstlösning](./media/cloud-services-python-ptvs/worker.png)

Du kan lägga till webb- eller arbetarroll roller tooan befintlig molntjänst när som helst.  Du kan välja tooadd befintliga projekt i din lösning eller skapa nya.

![Kommandot Lägg till roll](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Din molntjänst kan innehålla roller som implementeras på olika språk.  Exempelvis kan en Python-webbroll implementeras med hjälp av Django, med Python, eller med C#-arbetsroller.  Du kan enkelt kommunicera mellan dina roller med hjälp av Service Bus-köer eller lagringsköer.

## <a name="install-python-on-hello-cloud-service"></a>Installera Python på hello Molntjänsten
> [!WARNING]
> hello installationsskript som installeras med Visual Studio (för närvarande hello artikeln uppdaterades senast) fungerar inte. Det här avsnittet beskriver en lösning.
> 
> 

hello viktigaste problemet med hello installationsskript är att de inte installerar python. Först definierar två [Start uppgifter](cloud-services-startup-tasks.md) i hello [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) fil. hello första uppgiften (**PrepPython.ps1**) hämtar och installerar hello Python-körning. hello andra aktiviteten (**PipInstaller.ps1**) och kör pip tooinstall eventuella beroenden som du kan ha.

Följande skript hello skrevs riktad Python 3.5. Om du vill toouse hello version 2.x av python, ange hello **PYTHON2** variabeln filen för**på** för hello två Start aktiviteter och hello runtime uppgiften: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.

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

Hej **PYTHON2** och **PYPATH** toohello worker startaktivitet måste läggas till variabler. Hej **PYPATH** variabeln används bara om hello **PYTHON2** variabeln anges för**på**.

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

#### <a name="sample-servicedefinitioncsdef"></a>Exempel ServiceDefinition.csdef
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



Skapa sedan hello **PrepPython.ps1** och **PipInstaller.ps1** filer i hello **. / bin** mappen för din roll.

#### <a name="preppythonps1"></a>PrepPython.ps1
Det här skriptet installerar Python. Om hello **PYTHON2** miljövariabeln har ställts in för**på**, och sedan Python 2.7 är installerad, annars Python 3.5 är installerat.

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

#### <a name="pipinstallerps1"></a>PipInstaller.ps1
Det här skriptet ringer upp pip och installerar alla hello beroenden i hello **requirements.txt** fil. Om hello **PYTHON2** miljövariabeln har ställts in för**på**, Python 2.7 används, annars Python 3.5 används.

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

#### <a name="modify-launchworkerps1"></a>Ändra LaunchWorker.ps1
> [!NOTE]
> Hello gäller en **arbetsrollen** projektet **LauncherWorker.ps1** är obligatoriska tooexecute hello Start-fil. I en **webbroll** projekt, hello startfil i stället definieras i hello projektegenskaperna.
> 
> 

Hej **bin\LaunchWorker.ps1** ursprungligen skapades toodo mycket prep arbete, men det inte fungerar. Ersätt hello innehållet i filen med hello följande skript.

Det här skriptet anropar hello **worker.py** filen från projektet python. Om hello **PYTHON2** miljövariabeln har ställts in för**på**, Python 2.7 används, annars Python 3.5 används.

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

#### <a name="pscmd"></a>ps.cmd
hello Visual Studio mallar ha skapat en **ps.cmd** filen i hello **. / bin** mapp. Shell-skript visar hello PowerShell wrapper skript ovan och tillhandahåller loggning baserat på hello namn för hello PowerShell wrapper anropas. Detta är vad som bör finnas i den, om filen inte skapades. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Lokal körning
Om du konfigurera ditt molntjänstprojekt som hello Startprojekt och tryck på F5 körs Molntjänsten hello i hello lokala Azure-emulatorn.

Även om PTVS stöder fungerar startas i hello emulatorn felsökning (till exempel brytpunkter) inte.

toodebug dina webb- och arbetsroller roller som du kan ange hello rollen projektet som Startprojekt hello och felsöka det i stället.  Du kan också ange flera startprojekt.  Högerklicka på hello lösning och välj sedan **ange Startprojekt**.

![Egenskaper för lösningens startprojekt](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-tooazure"></a>Publicera tooAzure
toopublish, högerklicka på hello molntjänstprojektet i lösningen hello och välj sedan **publicera**.

![Inloggning för publicering i Microsoft Azure](./media/cloud-services-python-ptvs/publish-sign-in.png)

Följ guiden hello. Aktivera Fjärrskrivbord vid behov. Fjärrskrivbord är användbart när du behöver toodebug något.

När du har konfigurerat inställningarna klickar du på **Publicera**.

Förloppet visas i utdatafönstret hello och sedan visas hello Azure-aktivitetsloggen fönster.

![Fönstret med Microsoft Azure-aktivitetsloggen](./media/cloud-services-python-ptvs/publish-activity-log.png)

Distributionen tar flera minuter toocomplete, sedan webbplatsen och/eller arbetsroller köras i Azure!

### <a name="investigate-logs"></a>Undersöka loggar
När hello cloud service virtuell dator startar och installerar Python, kan du titta på hello loggar toofind eventuella felmeddelanden. Dessa loggar finns i hello **C:\Resources\Directory\\{rollen} \LogFiles** mapp. **PrepPython.err.txt** har minst ett fel i den från när hello-skriptet försöker toodetect om Python är installerad och **PipInstaller.err.txt** kan klagar på att en inaktuell version av pip.

## <a name="next-steps"></a>Nästa steg
Mer detaljerad information om hur du arbetar med webb- och arbetsroller roller i Python Tools för Visual Studio finns i dokumentationen till PTVS hello:

* [Molntjänstprojekt][Cloud Service Projects]

Mer information om hur du använder Azure-tjänster från dina webb- och arbetsroller roller, till exempel med hjälp av Azure Storage eller Azure Service Bus finns hello följande artiklar:

* [Blob Service][Blob Service]
* [Table Service][Table Service]
* [Kötjänst][Queue Service]
* [Service Bus-köer][Service Bus Queues]
* [Service Bus-avsnitt][Service Bus Topics]

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
