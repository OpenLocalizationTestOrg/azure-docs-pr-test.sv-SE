---
title: "aaaTroubleshooting Docker klientfel i Windows med hjälp av Visual Studio | Microsoft Docs"
description: "Felsöka problem som kan uppstå när du använder Visual Studio toocreate och distribuera web apps tooDocker i Windows med hjälp av Visual Studio."
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 7421ae8e044d58fc412d748fb870da4c9b2fdb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a><span data-ttu-id="c97da-103">Felsökning av Visual Studio Docker-utveckling</span><span class="sxs-lookup"><span data-stu-id="c97da-103">Troubleshoot Visual Studio Docker development</span></span>

<span data-ttu-id="c97da-104">När du arbetar med Visual Studio Tools för Docker Preview kan vissa problem uppstå på grund av hello uppbyggnad hello preview.</span><span class="sxs-lookup"><span data-stu-id="c97da-104">When you're working with Visual Studio Tools for Docker Preview, you may encounter some problems because of hello nature of hello preview.</span></span>
<span data-ttu-id="c97da-105">Följande är några vanliga problem och lösningar.</span><span class="sxs-lookup"><span data-stu-id="c97da-105">Following are some common issues and resolutions.</span></span>  

## <a name="visual-studio-2017-rc"></a><span data-ttu-id="c97da-106">Visual Studio 2017 RC</span><span class="sxs-lookup"><span data-stu-id="c97da-106">Visual Studio 2017 RC</span></span>

### <a name="linux-containers"></a><span data-ttu-id="c97da-107">**Linux-behållare**</span><span class="sxs-lookup"><span data-stu-id="c97da-107">**Linux containers**</span></span>

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a><span data-ttu-id="c97da-108">Skapa fel uppstår när du felsöker ett .NET Core webb- eller konsolen program</span><span class="sxs-lookup"><span data-stu-id="c97da-108">Build errors occur when debugging a .NET Core web or console application</span></span>  

<span data-ttu-id="c97da-109">Detta kan vara relaterade toonot dela hello enheten där hello projektet finns med Docker för Windows.</span><span class="sxs-lookup"><span data-stu-id="c97da-109">This could be related toonot sharing hello drive where hello project resides with Docker for Windows.</span></span>  <span data-ttu-id="c97da-110">Du kan få ett felmeddelande som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="c97da-110">You may receive an error like hello following:</span></span>

```
hello "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with hello default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
<span data-ttu-id="c97da-111">tooresolve problemet:</span><span class="sxs-lookup"><span data-stu-id="c97da-111">tooresolve this issue:</span></span>

1. <span data-ttu-id="c97da-112">Högerklicka på **Docker för Windows** i hello meddelandefältet och välj sedan **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c97da-112">Right-click **Docker for Windows** in hello notification area, and then select **Settings**.</span></span>  
2. <span data-ttu-id="c97da-113">Välj **delade enheter** och dela hello enhet där hello projektet finns.</span><span class="sxs-lookup"><span data-stu-id="c97da-113">Select **Shared Drives** and share hello drive where hello project resides.</span></span>

### <a name="windows-containers"></a><span data-ttu-id="c97da-114">**Windows-behållare**</span><span class="sxs-lookup"><span data-stu-id="c97da-114">**Windows containers**</span></span>

<span data-ttu-id="c97da-115">hello är följande problem särskilda toodebugging .NET Framework-webb- och konsolen program i Windows-behållare.</span><span class="sxs-lookup"><span data-stu-id="c97da-115">hello following issues are specific toodebugging .NET Framework web and console applications in Windows containers.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="c97da-116">Krav</span><span class="sxs-lookup"><span data-stu-id="c97da-116">Prerequisites</span></span>

1. <span data-ttu-id="c97da-117">Visual Studio 2017 RC (eller senare) med hello .NET Core och Docker Preview arbetsbelastning måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="c97da-117">Visual Studio 2017 RC (or later) with hello .NET Core and Docker Preview workload must be installed.</span></span>
2. <span data-ttu-id="c97da-118">Uppdatering av Windows 10 årsdagar med den senaste Windows Update korrigeringsfiler.</span><span class="sxs-lookup"><span data-stu-id="c97da-118">Windows 10 Anniversary Update with that latest Windows Update patches.</span></span> <span data-ttu-id="c97da-119">Mer specifikt [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="c97da-119">Specifically [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) must be installed.</span></span> 
3. <span data-ttu-id="c97da-120">[Docker för Windows](https://docs.docker.com/docker-for-windows/) (skapa 1.13.0 eller senare) måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="c97da-120">[Docker for Windows](https://docs.docker.com/docker-for-windows/) (build 1.13.0 or later) must be installed.</span></span>
4. <span data-ttu-id="c97da-121">**Växla tooWindows behållare** måste väljas.</span><span class="sxs-lookup"><span data-stu-id="c97da-121">**Switch tooWindows containers** must be selected.</span></span> <span data-ttu-id="c97da-122">I hello meddelandefältet klickar du på **Docker för Windows**, och välj sedan **växla tooWindows behållare**.</span><span class="sxs-lookup"><span data-stu-id="c97da-122">In hello notification area, click **Docker for Windows**, and then select **Switch tooWindows containers**.</span></span> <span data-ttu-id="c97da-123">Se till att den här inställningen sparas när hello datorn har startats om.</span><span class="sxs-lookup"><span data-stu-id="c97da-123">After hello machine restarts, ensure that this setting is retained.</span></span>

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a><span data-ttu-id="c97da-124">Konsolens utdata visas inte i Visual Studio utdata när du felsöker ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="c97da-124">Console output does not appear in Visual Studio's output window while debugging a console application</span></span>

<span data-ttu-id="c97da-125">Detta är ett känt problem med hello Visual Studio debugger (msvsmon.exe) som för närvarande inte för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="c97da-125">This is a known issue with hello Visual Studio debugger (msvsmon.exe), which is currently not designed for this scenario.</span></span> <span data-ttu-id="c97da-126">Stöd för det här scenariot kan ingå i framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="c97da-126">Support for this scenario might be included in a future release.</span></span> <span data-ttu-id="c97da-127">toosee utdata från hello konsolprogram i Visual Studio, Använd **Docker: starta projektet**, vilket motsvarar för**starta utan Debugging**.</span><span class="sxs-lookup"><span data-stu-id="c97da-127">toosee output from hello console application in Visual Studio, use **Docker: Start Project**, which is equivalent too**Start without Debugging**.</span></span>

#### <a name="debugging-web-applications-with-hello-release-configuration-fails-with-403-forbidden-error"></a><span data-ttu-id="c97da-128">Felsökning webbprogram med hello släpper konfigurationen misslyckas med fel (403) förbjuden</span><span class="sxs-lookup"><span data-stu-id="c97da-128">Debugging web applications with hello release configuration fails with (403) Forbidden error</span></span>

<span data-ttu-id="c97da-129">toowork runt det här problemet öppnar web.release.config i hello lösning och kommentera ut eller ta bort hello följande rader:</span><span class="sxs-lookup"><span data-stu-id="c97da-129">toowork around this issue, open web.release.config in hello solution and comment out or delete hello following lines:</span></span>

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a><span data-ttu-id="c97da-130">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="c97da-130">Visual Studio 2015</span></span>

### <a name="linux-containers"></a><span data-ttu-id="c97da-131">**Linux-behållare**</span><span class="sxs-lookup"><span data-stu-id="c97da-131">**Linux containers**</span></span>

#### <a name="unable-toovalidate-volume-mapping"></a><span data-ttu-id="c97da-132">Det går inte toovalidate volym mappning</span><span class="sxs-lookup"><span data-stu-id="c97da-132">Unable toovalidate volume mapping</span></span>
<span data-ttu-id="c97da-133">Volymen mappningen är obligatoriska tooshare hello källkoden och binärfilerna för ditt program med hello app mappen i hello behållaren.</span><span class="sxs-lookup"><span data-stu-id="c97da-133">Volume mapping is required tooshare hello source code and binaries of your application with hello app folder in hello container.</span></span>  <span data-ttu-id="c97da-134">Specifika volymen mappningar ingår i docker-compose.dev.debug.yml och docker-compose.dev.release.yml.</span><span class="sxs-lookup"><span data-stu-id="c97da-134">Specific volume mappings are contained within docker-compose.dev.debug.yml and docker-compose.dev.release.yml.</span></span> <span data-ttu-id="c97da-135">När filer ändras på värddatorn, de här ändringarna i en liknande mappstruktur hello behållarna.</span><span class="sxs-lookup"><span data-stu-id="c97da-135">As files are changed on your host machine, hello containers reflect these changes in a similar folder structure.</span></span>

<span data-ttu-id="c97da-136">tooenable volym mappning:</span><span class="sxs-lookup"><span data-stu-id="c97da-136">tooenable volume mapping:</span></span>

1. <span data-ttu-id="c97da-137">Klicka på **Moby** i hello meddelandefältet och välj **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c97da-137">Click **Moby** in hello notification area and select **Settings**.</span></span>
2. <span data-ttu-id="c97da-138">Välj **delade enheter**.</span><span class="sxs-lookup"><span data-stu-id="c97da-138">Select **Shared Drives**.</span></span>
3. <span data-ttu-id="c97da-139">Välj hello-enhet som är värd för ditt projekt och hello enhet där % USERPROFILE % finns.</span><span class="sxs-lookup"><span data-stu-id="c97da-139">Select hello drive that hosts your project and hello drive where %USERPROFILE% resides.</span></span>
4. <span data-ttu-id="c97da-140">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="c97da-140">Click **Apply**.</span></span>

<span data-ttu-id="c97da-141">tootest om volymen mappning fungerar återskapa och väljer F5 från Visual Studio när en eller flera enheter har delat eller kör hello följande kod från en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="c97da-141">tootest if volume mapping is functioning, Rebuild and select F5 from within Visual Studio after one or more drives have been shared, or run hello following code from a command prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="c97da-142">Det här exemplet förutsätter att mappen användare finns på enhet C och att den har delats.</span><span class="sxs-lookup"><span data-stu-id="c97da-142">This example assumes that your Users folder is located on drive C and that it has been shared.</span></span>
> <span data-ttu-id="c97da-143">Ändra vid behov om du har delat en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="c97da-143">Revise as necessary if you have shared a different drive.</span></span>

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

<span data-ttu-id="c97da-144">Kör hello följande kod i hello Linux behållaren.</span><span class="sxs-lookup"><span data-stu-id="c97da-144">Run hello following code in hello Linux container.</span></span>

```
/ # ls
```

<span data-ttu-id="c97da-145">Du bör se en kataloglista från hello användare/offentlig mapp.</span><span class="sxs-lookup"><span data-stu-id="c97da-145">You should see a directory listing from hello Users/Public folder.</span></span> <span data-ttu-id="c97da-146">Om inga filer visas och mappen /c/Users/Public inte är tom, är volym mappning inte korrekt konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="c97da-146">If no files are displayed and your /c/Users/Public folder isn't empty, volume mapping is not configured properly.</span></span>

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

<span data-ttu-id="c97da-147">Ändra toohello destinationsindikeringen directory toosee hello innehåll hello `/c/Users/Public` directory:</span><span class="sxs-lookup"><span data-stu-id="c97da-147">Change toohello wormhole directory toosee hello contents of hello `/c/Users/Public` directory:</span></span>

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> <span data-ttu-id="c97da-148">När du arbetar med virtuella Linux-datorer är hello behållaren filsystemet skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="c97da-148">When you're working with Linux VMs, hello container file system is case-sensitive.</span></span>

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a><span data-ttu-id="c97da-149">Build: ”PrepareForBuild” aktiviteten misslyckades oväntat</span><span class="sxs-lookup"><span data-stu-id="c97da-149">Build: "PrepareForBuild" task failed unexpectedly</span></span>

<span data-ttu-id="c97da-150">Microsoft.DotNet.Docker.CommandLine.ClientException: Ett fel uppstod när tooconnect.</span><span class="sxs-lookup"><span data-stu-id="c97da-150">Microsoft.DotNet.Docker.CommandLine.ClientException: An error occurred trying tooconnect.</span></span>

<span data-ttu-id="c97da-151">Kontrollera att hello standard Docker-värden körs.</span><span class="sxs-lookup"><span data-stu-id="c97da-151">Verify that hello default Docker host is running.</span></span> <span data-ttu-id="c97da-152">Öppna en kommandotolk och kör:</span><span class="sxs-lookup"><span data-stu-id="c97da-152">Open a command prompt and execute:</span></span>

```
docker info
```

<span data-ttu-id="c97da-153">Om detta returnerar ett fel och försök sedan toostart hello **Docker för Windows** skrivbordsapp.</span><span class="sxs-lookup"><span data-stu-id="c97da-153">If this returns an error, then attempt toostart hello **Docker for Windows** desktop app.</span></span> <span data-ttu-id="c97da-154">Om hello skrivbordsapp körs sedan **Moby** ska visas i meddelandefältet hello.</span><span class="sxs-lookup"><span data-stu-id="c97da-154">If hello desktop app is running, then **Moby** should be visible in hello notification area.</span></span> <span data-ttu-id="c97da-155">Högerklicka på **Moby** och öppna **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="c97da-155">Right-click **Moby** and open **Settings**.</span></span> <span data-ttu-id="c97da-156">Klicka på **återställa**, och starta sedan om Docker.</span><span class="sxs-lookup"><span data-stu-id="c97da-156">Click **Reset**, and then restart Docker.</span></span>

## <a name="an-error-dialog-occurs-when-attempting-tooadd-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a><span data-ttu-id="c97da-157">En feldialogruta inträffar när försök tooadd Docker Support eller felsöka ett ASP.NET Core program i en behållare för (F5)</span><span class="sxs-lookup"><span data-stu-id="c97da-157">An error dialog occurs when attempting tooadd Docker Support or debug (F5) an ASP.NET Core application in a container</span></span>

<span data-ttu-id="c97da-158">När du avinstallerar och installerar tillägg, kan hello hanteras utökningsbarhet Framework (MEF) cache i Visual Studio skadas.</span><span class="sxs-lookup"><span data-stu-id="c97da-158">After uninstalling and installing extensions, hello Managed Extensibility Framework (MEF) cache in Visual Studio can become corrupted.</span></span> <span data-ttu-id="c97da-159">När detta inträffar kan det leda till olika felmeddelanden när du lägga till stöd för Docker och/eller försök toorun eller felsöka (F5) ASP.NET Core programmet.</span><span class="sxs-lookup"><span data-stu-id="c97da-159">When this occurs, it can cause various error messages when you're adding Docker Support and/or attempting toorun or debug (F5) your ASP.NET Core application.</span></span> <span data-ttu-id="c97da-160">Som en tillfällig lösning kan använda hello följande steg toodelete och generera hello MEF cache.</span><span class="sxs-lookup"><span data-stu-id="c97da-160">As a temporary workaround, use hello following steps toodelete and regenerate hello MEF cache.</span></span>

1. <span data-ttu-id="c97da-161">Stäng alla instanser av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c97da-161">Close all instances of Visual Studio.</span></span>
1. <span data-ttu-id="c97da-162">Öppna % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.</span><span class="sxs-lookup"><span data-stu-id="c97da-162">Open %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.</span></span>
1. <span data-ttu-id="c97da-163">Ta bort hello följande mappar:</span><span class="sxs-lookup"><span data-stu-id="c97da-163">Delete hello following folders:</span></span>
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. <span data-ttu-id="c97da-164">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c97da-164">Open Visual Studio.</span></span>
1. <span data-ttu-id="c97da-165">Försök hello scenariot igen.</span><span class="sxs-lookup"><span data-stu-id="c97da-165">Attempt hello scenario again.</span></span>
