---
title: "Felsökning Docker-klienten i Windows med hjälp av Visual Studio | Microsoft Docs"
description: "Felsöka problem som kan uppstå när du använder Visual Studio för att skapa och distribuera webbprogram till Docker i Windows med hjälp av Visual Studio."
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
ms.openlocfilehash: 89fa04a1107b6abb49aefd68066443717ac9b731
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a><span data-ttu-id="8aaae-103">Felsökning av Visual Studio Docker-utveckling</span><span class="sxs-lookup"><span data-stu-id="8aaae-103">Troubleshoot Visual Studio Docker development</span></span>

<span data-ttu-id="8aaae-104">När du arbetar med Visual Studio Tools för Docker Preview kan vissa problem uppstå på grund av arten av förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="8aaae-104">When you're working with Visual Studio Tools for Docker Preview, you may encounter some problems because of the nature of the preview.</span></span>
<span data-ttu-id="8aaae-105">Följande är några vanliga problem och lösningar.</span><span class="sxs-lookup"><span data-stu-id="8aaae-105">Following are some common issues and resolutions.</span></span>  

## <a name="visual-studio-2017-rc"></a><span data-ttu-id="8aaae-106">Visual Studio 2017 RC</span><span class="sxs-lookup"><span data-stu-id="8aaae-106">Visual Studio 2017 RC</span></span>

### <a name="linux-containers"></a><span data-ttu-id="8aaae-107">**Linux-behållare**</span><span class="sxs-lookup"><span data-stu-id="8aaae-107">**Linux containers**</span></span>

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a><span data-ttu-id="8aaae-108">Skapa fel uppstår när du felsöker ett .NET Core webb- eller konsolen program</span><span class="sxs-lookup"><span data-stu-id="8aaae-108">Build errors occur when debugging a .NET Core web or console application</span></span>  

<span data-ttu-id="8aaae-109">Detta kan vara relaterat till inte dela enheten där projektet finns med Docker för Windows.</span><span class="sxs-lookup"><span data-stu-id="8aaae-109">This could be related to not sharing the drive where the project resides with Docker for Windows.</span></span>  <span data-ttu-id="8aaae-110">Du kan få ett felmeddelande som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="8aaae-110">You may receive an error like the following:</span></span>

```
The "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with the default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
<span data-ttu-id="8aaae-111">Så här löser du problemet:</span><span class="sxs-lookup"><span data-stu-id="8aaae-111">To resolve this issue:</span></span>

1. <span data-ttu-id="8aaae-112">Högerklicka på **Docker för Windows** i meddelandefältet och välj sedan **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="8aaae-112">Right-click **Docker for Windows** in the notification area, and then select **Settings**.</span></span>  
2. <span data-ttu-id="8aaae-113">Välj **delade enheter** och dela den enhet där projektet finns.</span><span class="sxs-lookup"><span data-stu-id="8aaae-113">Select **Shared Drives** and share the drive where the project resides.</span></span>

### <a name="windows-containers"></a><span data-ttu-id="8aaae-114">**Windows-behållare**</span><span class="sxs-lookup"><span data-stu-id="8aaae-114">**Windows containers**</span></span>

<span data-ttu-id="8aaae-115">Följande är specifika för felsökning av .NET Framework-webb- och konsolen program i Windows-behållare.</span><span class="sxs-lookup"><span data-stu-id="8aaae-115">The following issues are specific to debugging .NET Framework web and console applications in Windows containers.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="8aaae-116">Krav</span><span class="sxs-lookup"><span data-stu-id="8aaae-116">Prerequisites</span></span>

1. <span data-ttu-id="8aaae-117">Visual Studio 2017 RC (eller senare) med .NET Core och Docker Preview arbetsbelastning måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="8aaae-117">Visual Studio 2017 RC (or later) with the .NET Core and Docker Preview workload must be installed.</span></span>
2. <span data-ttu-id="8aaae-118">Uppdatering av Windows 10 årsdagar med den senaste Windows Update korrigeringsfiler.</span><span class="sxs-lookup"><span data-stu-id="8aaae-118">Windows 10 Anniversary Update with that latest Windows Update patches.</span></span> <span data-ttu-id="8aaae-119">Mer specifikt [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="8aaae-119">Specifically [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) must be installed.</span></span> 
3. <span data-ttu-id="8aaae-120">[Docker för Windows](https://docs.docker.com/docker-for-windows/) (skapa 1.13.0 eller senare) måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="8aaae-120">[Docker for Windows](https://docs.docker.com/docker-for-windows/) (build 1.13.0 or later) must be installed.</span></span>
4. <span data-ttu-id="8aaae-121">**Växla till Windows-behållare** måste väljas.</span><span class="sxs-lookup"><span data-stu-id="8aaae-121">**Switch to Windows containers** must be selected.</span></span> <span data-ttu-id="8aaae-122">I meddelandefältet klickar du på **Docker för Windows**, och välj sedan **växla till Windows-behållare**.</span><span class="sxs-lookup"><span data-stu-id="8aaae-122">In the notification area, click **Docker for Windows**, and then select **Switch to Windows containers**.</span></span> <span data-ttu-id="8aaae-123">Se till att den här inställningen sparas när datorn startas om.</span><span class="sxs-lookup"><span data-stu-id="8aaae-123">After the machine restarts, ensure that this setting is retained.</span></span>

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a><span data-ttu-id="8aaae-124">Konsolens utdata visas inte i Visual Studio utdata när du felsöker ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="8aaae-124">Console output does not appear in Visual Studio's output window while debugging a console application</span></span>

<span data-ttu-id="8aaae-125">Detta är ett känt problem med Visual Studio-felsökaren (msvsmon.exe) som för närvarande inte för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="8aaae-125">This is a known issue with the Visual Studio debugger (msvsmon.exe), which is currently not designed for this scenario.</span></span> <span data-ttu-id="8aaae-126">Stöd för det här scenariot kan ingå i framtida versioner.</span><span class="sxs-lookup"><span data-stu-id="8aaae-126">Support for this scenario might be included in a future release.</span></span> <span data-ttu-id="8aaae-127">Utdata från konsolprogram i Visual Studio, Använd **Docker: starta projektet**, vilket motsvarar **starta utan Debugging**.</span><span class="sxs-lookup"><span data-stu-id="8aaae-127">To see output from the console application in Visual Studio, use **Docker: Start Project**, which is equivalent to **Start without Debugging**.</span></span>

#### <a name="debugging-web-applications-with-the-release-configuration-fails-with-403-forbidden-error"></a><span data-ttu-id="8aaae-128">Felsökning av webbprogram med versionen av konfigurationen inte lyckas med (403) förbjuden-fel</span><span class="sxs-lookup"><span data-stu-id="8aaae-128">Debugging web applications with the release configuration fails with (403) Forbidden error</span></span>

<span data-ttu-id="8aaae-129">Undvik problemet genom att öppna web.release.config i lösningen och kommentera ut eller ta bort följande rader:</span><span class="sxs-lookup"><span data-stu-id="8aaae-129">To work around this issue, open web.release.config in the solution and comment out or delete the following lines:</span></span>

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a><span data-ttu-id="8aaae-130">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="8aaae-130">Visual Studio 2015</span></span>

### <a name="linux-containers"></a><span data-ttu-id="8aaae-131">**Linux-behållare**</span><span class="sxs-lookup"><span data-stu-id="8aaae-131">**Linux containers**</span></span>

#### <a name="unable-to-validate-volume-mapping"></a><span data-ttu-id="8aaae-132">Det gick inte att verifiera mappningen för volym</span><span class="sxs-lookup"><span data-stu-id="8aaae-132">Unable to validate volume mapping</span></span>
<span data-ttu-id="8aaae-133">Mappning av volym krävs för att dela källkoden och binärfilerna för ditt program med app-mappen i behållaren.</span><span class="sxs-lookup"><span data-stu-id="8aaae-133">Volume mapping is required to share the source code and binaries of your application with the app folder in the container.</span></span>  <span data-ttu-id="8aaae-134">Specifika volymen mappningar ingår i docker-compose.dev.debug.yml och docker-compose.dev.release.yml.</span><span class="sxs-lookup"><span data-stu-id="8aaae-134">Specific volume mappings are contained within docker-compose.dev.debug.yml and docker-compose.dev.release.yml.</span></span> <span data-ttu-id="8aaae-135">När filer ändras på värddatorn, de här ändringarna i en liknande mappstruktur behållarna.</span><span class="sxs-lookup"><span data-stu-id="8aaae-135">As files are changed on your host machine, the containers reflect these changes in a similar folder structure.</span></span>

<span data-ttu-id="8aaae-136">Aktivera mappning för volymen:</span><span class="sxs-lookup"><span data-stu-id="8aaae-136">To enable volume mapping:</span></span>

1. <span data-ttu-id="8aaae-137">Klicka på **Moby** i meddelandefältet och välj **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="8aaae-137">Click **Moby** in the notification area and select **Settings**.</span></span>
2. <span data-ttu-id="8aaae-138">Välj **delade enheter**.</span><span class="sxs-lookup"><span data-stu-id="8aaae-138">Select **Shared Drives**.</span></span>
3. <span data-ttu-id="8aaae-139">Välj den enhet som är värd för ditt projekt och den enhet där % USERPROFILE % finns.</span><span class="sxs-lookup"><span data-stu-id="8aaae-139">Select the drive that hosts your project and the drive where %USERPROFILE% resides.</span></span>
4. <span data-ttu-id="8aaae-140">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="8aaae-140">Click **Apply**.</span></span>

<span data-ttu-id="8aaae-141">Om du vill testa om volymen mappning fungerar, återskapa och välj F5 från Visual Studio när en eller flera enheter har delat eller kör följande kod från en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="8aaae-141">To test if volume mapping is functioning, Rebuild and select F5 from within Visual Studio after one or more drives have been shared, or run the following code from a command prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="8aaae-142">Det här exemplet förutsätter att mappen användare finns på enhet C och att den har delats.</span><span class="sxs-lookup"><span data-stu-id="8aaae-142">This example assumes that your Users folder is located on drive C and that it has been shared.</span></span>
> <span data-ttu-id="8aaae-143">Ändra vid behov om du har delat en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="8aaae-143">Revise as necessary if you have shared a different drive.</span></span>

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

<span data-ttu-id="8aaae-144">Kör följande kod i Linux-behållaren.</span><span class="sxs-lookup"><span data-stu-id="8aaae-144">Run the following code in the Linux container.</span></span>

```
/ # ls
```

<span data-ttu-id="8aaae-145">Du bör se en kataloglista från mappen användare och offentliga.</span><span class="sxs-lookup"><span data-stu-id="8aaae-145">You should see a directory listing from the Users/Public folder.</span></span> <span data-ttu-id="8aaae-146">Om inga filer visas och mappen /c/Users/Public inte är tom, är volym mappning inte korrekt konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="8aaae-146">If no files are displayed and your /c/Users/Public folder isn't empty, volume mapping is not configured properly.</span></span>

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

<span data-ttu-id="8aaae-147">Ändra till katalogen destinationsindikeringen för att visa innehållet i den `/c/Users/Public` directory:</span><span class="sxs-lookup"><span data-stu-id="8aaae-147">Change to the wormhole directory to see the contents of the `/c/Users/Public` directory:</span></span>

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> <span data-ttu-id="8aaae-148">När du arbetar med virtuella Linux-datorer, är behållare filsystemet skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="8aaae-148">When you're working with Linux VMs, the container file system is case-sensitive.</span></span>

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a><span data-ttu-id="8aaae-149">Build: ”PrepareForBuild” aktiviteten misslyckades oväntat</span><span class="sxs-lookup"><span data-stu-id="8aaae-149">Build: "PrepareForBuild" task failed unexpectedly</span></span>

<span data-ttu-id="8aaae-150">Microsoft.DotNet.Docker.CommandLine.ClientException: Ett fel uppstod vid försök att ansluta.</span><span class="sxs-lookup"><span data-stu-id="8aaae-150">Microsoft.DotNet.Docker.CommandLine.ClientException: An error occurred trying to connect.</span></span>

<span data-ttu-id="8aaae-151">Kontrollera att standard Docker-värden körs.</span><span class="sxs-lookup"><span data-stu-id="8aaae-151">Verify that the default Docker host is running.</span></span> <span data-ttu-id="8aaae-152">Öppna en kommandotolk och kör:</span><span class="sxs-lookup"><span data-stu-id="8aaae-152">Open a command prompt and execute:</span></span>

```
docker info
```

<span data-ttu-id="8aaae-153">Om ett fel returneras, försöker starta den **Docker för Windows** skrivbordsapp.</span><span class="sxs-lookup"><span data-stu-id="8aaae-153">If this returns an error, then attempt to start the **Docker for Windows** desktop app.</span></span> <span data-ttu-id="8aaae-154">Om skrivbordsappen körs sedan **Moby** ska visas i meddelandefältet.</span><span class="sxs-lookup"><span data-stu-id="8aaae-154">If the desktop app is running, then **Moby** should be visible in the notification area.</span></span> <span data-ttu-id="8aaae-155">Högerklicka på **Moby** och öppna **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="8aaae-155">Right-click **Moby** and open **Settings**.</span></span> <span data-ttu-id="8aaae-156">Klicka på **återställa**, och starta sedan om Docker.</span><span class="sxs-lookup"><span data-stu-id="8aaae-156">Click **Reset**, and then restart Docker.</span></span>

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a><span data-ttu-id="8aaae-157">En feldialogruta uppstår vid försök att lägga till stöd för Docker eller felsöka ett ASP.NET Core program i en behållare för (F5)</span><span class="sxs-lookup"><span data-stu-id="8aaae-157">An error dialog occurs when attempting to add Docker Support or debug (F5) an ASP.NET Core application in a container</span></span>

<span data-ttu-id="8aaae-158">När du avinstallerar och installerar tillägg, kan hanteras utökningsbarhet Framework (MEF)-cachen i Visual Studio skadas.</span><span class="sxs-lookup"><span data-stu-id="8aaae-158">After uninstalling and installing extensions, the Managed Extensibility Framework (MEF) cache in Visual Studio can become corrupted.</span></span> <span data-ttu-id="8aaae-159">När detta inträffar kan det orsaka olika felmeddelanden när du lägga till stöd för Docker och/eller försök att köra eller felsöka (F5) ASP.NET Core programmet.</span><span class="sxs-lookup"><span data-stu-id="8aaae-159">When this occurs, it can cause various error messages when you're adding Docker Support and/or attempting to run or debug (F5) your ASP.NET Core application.</span></span> <span data-ttu-id="8aaae-160">Som en tillfällig lösning kan använda följande steg att ta bort och återskapa MEF-cachen.</span><span class="sxs-lookup"><span data-stu-id="8aaae-160">As a temporary workaround, use the following steps to delete and regenerate the MEF cache.</span></span>

1. <span data-ttu-id="8aaae-161">Stäng alla instanser av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8aaae-161">Close all instances of Visual Studio.</span></span>
1. <span data-ttu-id="8aaae-162">Öppna % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.</span><span class="sxs-lookup"><span data-stu-id="8aaae-162">Open %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.</span></span>
1. <span data-ttu-id="8aaae-163">Ta bort följande mappar:</span><span class="sxs-lookup"><span data-stu-id="8aaae-163">Delete the following folders:</span></span>
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. <span data-ttu-id="8aaae-164">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8aaae-164">Open Visual Studio.</span></span>
1. <span data-ttu-id="8aaae-165">Försök scenariot igen.</span><span class="sxs-lookup"><span data-stu-id="8aaae-165">Attempt the scenario again.</span></span>
