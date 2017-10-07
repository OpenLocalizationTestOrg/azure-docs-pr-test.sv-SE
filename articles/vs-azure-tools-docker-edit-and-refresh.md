---
title: "aaaDebugging appar i en lokal dockerbehållare | Microsoft Docs"
description: "Lär dig hur toomodify en app som körs i en lokal dockerbehållare uppdatera hello behållare via Redigera och uppdatera och ange brytpunkter-felsökning"
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 480e3062-aae7-48ef-9701-e4f9ea041382
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 07/22/2016
ms.author: mlearned
ms.openlocfilehash: ff64e62fbb93901a29b5496bd5e17d2c4ea5ca99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a><span data-ttu-id="bbedf-103">Felsök appar i en lokal Docker-behållare</span><span class="sxs-lookup"><span data-stu-id="bbedf-103">Debugging apps in a local Docker container</span></span>
## <a name="overview"></a><span data-ttu-id="bbedf-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="bbedf-104">Overview</span></span>
<span data-ttu-id="bbedf-105">hello Visual Studio Tools för Docker ger ett konsekvent sätt toodevelop i och validera ditt program lokalt i en Linux-dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="bbedf-105">hello Visual Studio Tools for Docker provides a consistent way toodevelop in and validate your application locally in a Linux Docker container.</span></span>
<span data-ttu-id="bbedf-106">Du har inte toorestart hello behållaren varje gång du gör en kod ändra.</span><span class="sxs-lookup"><span data-stu-id="bbedf-106">You don't have toorestart hello container each time you make a code change.</span></span>
<span data-ttu-id="bbedf-107">Den här artikeln visar hur toouse hello ”redigera och uppdatera” funktionen toostart en ASP.NET Core Web app i en lokal dockerbehållare gör nödvändiga ändringar och sedan uppdatera hello webbläsare toosee ändringarna.</span><span class="sxs-lookup"><span data-stu-id="bbedf-107">This article illustrates how toouse hello "Edit and Refresh" feature toostart an ASP.NET Core Web app in a local Docker container, make any necessary changes, and then refresh hello browser toosee those changes.</span></span>
<span data-ttu-id="bbedf-108">Den här artikeln visar också hur tooset brytpunkter för felsökning.</span><span class="sxs-lookup"><span data-stu-id="bbedf-108">This article also shows you how tooset breakpoints for debugging.</span></span>

> [!NOTE]
> <span data-ttu-id="bbedf-109">Stöd för Windows-behållare kommer i en framtida version</span><span class="sxs-lookup"><span data-stu-id="bbedf-109">Windows Container support will be coming in a future release</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="bbedf-110">Krav</span><span class="sxs-lookup"><span data-stu-id="bbedf-110">Prerequisites</span></span>
<span data-ttu-id="bbedf-111">hello följande verktyg måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="bbedf-111">hello following tools must be installed.</span></span>

* [<span data-ttu-id="bbedf-112">Senaste version av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bbedf-112">Latest version of Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="bbedf-113">Microsoft ASP.NET Core 1.0 SDK</span><span class="sxs-lookup"><span data-stu-id="bbedf-113">Microsoft ASP.NET Core 1.0 SDK</span></span>](https://go.microsoft.com/fwlink/?LinkID=809122)

<span data-ttu-id="bbedf-114">lokalt toorun Docker behållare, behöver du en lokal docker-klient.</span><span class="sxs-lookup"><span data-stu-id="bbedf-114">toorun Docker containers locally, you'll need a local docker client.</span></span>
<span data-ttu-id="bbedf-115">Du kan använda hello [Docker verktygslådan](https://www.docker.com/products/docker-toolbox), vilket kräver att Hyper-V toobe inaktiveras eller du kan använda [Docker för Windows](https://www.docker.com/get-docker), som använder Hyper-V och kräver att Windows 10.</span><span class="sxs-lookup"><span data-stu-id="bbedf-115">You can use hello [Docker Toolbox](https://www.docker.com/products/docker-toolbox), which requires Hyper-V toobe disabled, or you can use [Docker for Windows](https://www.docker.com/get-docker), which uses Hyper-V, and requires Windows 10.</span></span>

<span data-ttu-id="bbedf-116">Om du använder Docker verktygslådan, du behöver för[konfigurera hello Docker-klienten](vs-azure-tools-docker-setup.md)</span><span class="sxs-lookup"><span data-stu-id="bbedf-116">If using Docker Toolbox, you'll need too[configure hello Docker client](vs-azure-tools-docker-setup.md)</span></span>

## <a name="1-create-a-web-app"></a><span data-ttu-id="bbedf-117">1. Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="bbedf-117">1. Create a web app</span></span>
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="bbedf-118">2. Lägga till Docker-stöd</span><span class="sxs-lookup"><span data-stu-id="bbedf-118">2. Add Docker support</span></span>
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a><span data-ttu-id="bbedf-119">3. Redigera koden och uppdatera</span><span class="sxs-lookup"><span data-stu-id="bbedf-119">3. Edit your code and refresh</span></span>
<span data-ttu-id="bbedf-120">tooquickly iterera ändringar, kan du starta appen i en behållare och fortsätta toomake ändringar, visa dem precis som med IIS Express.</span><span class="sxs-lookup"><span data-stu-id="bbedf-120">tooquickly iterate changes, you can start your application within a container, and continue toomake changes, viewing them as you would with IIS Express.</span></span>

1. <span data-ttu-id="bbedf-121">Ange hello lösning konfiguration för`Debug` och tryck på  **&lt;CTRL + F5 >** toobuild din docker avbildningen och kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="bbedf-121">Set hello Solution Configuration too`Debug` and press **&lt;CTRL + F5>** toobuild your docker image and run it locally.</span></span>

    <span data-ttu-id="bbedf-122">När hello behållaren avbildningen har skapats och körs i en dockerbehållare, startas Visual Studio hello webbprogram i din standardwebbläsare.</span><span class="sxs-lookup"><span data-stu-id="bbedf-122">Once hello container image has been built and is running in a Docker container, Visual Studio will launch hello Web app in your default browser.</span></span>
    <span data-ttu-id="bbedf-123">Om du använder hello Microsoft Edge-webbläsaren eller på annat sätt har fel finns [felsökning](vs-azure-tools-docker-troubleshooting-docker-errors.md) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bbedf-123">If you are using hello Microsoft Edge browser or otherwise have errors, see [Troubleshooting](vs-azure-tools-docker-troubleshooting-docker-errors.md) section.</span></span>
2. <span data-ttu-id="bbedf-124">Gå toohello om sida, vilket är där vi toomake våra ändringar.</span><span class="sxs-lookup"><span data-stu-id="bbedf-124">Go toohello About page, which is where we're going toomake our changes.</span></span>
3. <span data-ttu-id="bbedf-125">Returnera tooVisual Studio och öppna `Views\Home\About.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="bbedf-125">Return tooVisual Studio and open `Views\Home\About.cshtml`.</span></span>
4. <span data-ttu-id="bbedf-126">Lägg till hello efter HTML-innehåll toohello hello-filen och spara hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="bbedf-126">Add hello following HTML content toohello end of hello file and save hello changes.</span></span>

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. <span data-ttu-id="bbedf-127">Visar hello utdatafönstret när hello .NET version har slutförts och du ser dessa rader kan växla tillbaka tooyour webbläsare och uppdatera hello om sida.</span><span class="sxs-lookup"><span data-stu-id="bbedf-127">Viewing hello output window, when hello .NET build is completed and you see these lines, switch back tooyour browser and refresh hello About page.</span></span>

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C tooshut down
   ```
6. <span data-ttu-id="bbedf-128">Ändringarna har tillämpats!</span><span class="sxs-lookup"><span data-stu-id="bbedf-128">Your changes have been applied!</span></span>

## <a name="4-debug-with-breakpoints"></a><span data-ttu-id="bbedf-129">4. Felsöka med brytpunkter</span><span class="sxs-lookup"><span data-stu-id="bbedf-129">4. Debug with breakpoints</span></span>
<span data-ttu-id="bbedf-130">Ofta behöver ändringar ytterligare kontroll, utnyttja hello felsökning funktioner i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bbedf-130">Often, changes will need further inspection, leveraging hello debugging features of Visual Studio.</span></span>

1. <span data-ttu-id="bbedf-131">Returnera tooVisual Studio och öppna`Controllers\HomeController.cs`</span><span class="sxs-lookup"><span data-stu-id="bbedf-131">Return tooVisual Studio and open `Controllers\HomeController.cs`</span></span>
2. <span data-ttu-id="bbedf-132">Ersätt hello innehållet i hello About() metod med hello följande:</span><span class="sxs-lookup"><span data-stu-id="bbedf-132">Replace hello contents of hello About() method with hello following:</span></span>

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. <span data-ttu-id="bbedf-133">Ange en brytpunkt toohello vänsterkant hello `string message`... rad.</span><span class="sxs-lookup"><span data-stu-id="bbedf-133">Set a breakpoint toohello left of hello `string message`... line.</span></span>
4. <span data-ttu-id="bbedf-134">Träffa  **&lt;F5 >** toostart felsökning.</span><span class="sxs-lookup"><span data-stu-id="bbedf-134">Hit **&lt;F5>** toostart debugging.</span></span>
5. <span data-ttu-id="bbedf-135">Navigera toohello om sidan toohit din brytpunkt.</span><span class="sxs-lookup"><span data-stu-id="bbedf-135">Navigate toohello About page toohit your breakpoint.</span></span>
6. <span data-ttu-id="bbedf-136">Växla tooVisual Studio tooview hello brytpunkt och inspektera hello-värdet för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="bbedf-136">Switch tooVisual Studio tooview hello breakpoint, and inspect hello value of message.</span></span>

   ![][2]

## <a name="summary"></a><span data-ttu-id="bbedf-137">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="bbedf-137">Summary</span></span>
<span data-ttu-id="bbedf-138">Med [Visual Studio 2015 Tools för Docker](https://aka.ms/DockerToolsForVS), du kan hämta hello produktivitet arbeta lokalt, med hello produktion mer att utveckla i en dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="bbedf-138">With [Visual Studio 2015 Tools for Docker](https://aka.ms/DockerToolsForVS), you can get hello productivity of working locally, with hello production realism of developing within a Docker container.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="bbedf-139">Felsökning</span><span class="sxs-lookup"><span data-stu-id="bbedf-139">Troubleshooting</span></span>
[<span data-ttu-id="bbedf-140">Felsökning av Visual Studio Docker-utveckling</span><span class="sxs-lookup"><span data-stu-id="bbedf-140">Troubleshooting Visual Studio Docker Development</span></span>](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a><span data-ttu-id="bbedf-141">Mer information om Docker med Visual Studio och Windows Azure</span><span class="sxs-lookup"><span data-stu-id="bbedf-141">More about Docker with Visual Studio, Windows, and Azure</span></span>
* <span data-ttu-id="bbedf-142">[Docker Tools för Visual Studio](http://aka.ms/dockertoolsforvs) -utveckla .NET Core koden i en behållare</span><span class="sxs-lookup"><span data-stu-id="bbedf-142">[Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - Developing your .NET Core code in a container</span></span>
* <span data-ttu-id="bbedf-143">[Docker-verktyg för Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) – skapa och distribuera docker-behållare</span><span class="sxs-lookup"><span data-stu-id="bbedf-143">[Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Build and Deploy docker containers</span></span>
* <span data-ttu-id="bbedf-144">[Docker-verktyg för Visual Studio Code](http://aka.ms/dockertoolsforvscode) -språk services för att redigera docker-filer, med mer e2e scenarier kommer</span><span class="sxs-lookup"><span data-stu-id="bbedf-144">[Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) - Language services for editing docker files, with more e2e scenarios coming</span></span>
* <span data-ttu-id="bbedf-145">[Information om Windows behållaren](http://aka.ms/containers)-information för Windows Server och Nano Server</span><span class="sxs-lookup"><span data-stu-id="bbedf-145">[Windows Container Information](http://aka.ms/containers)- Windows Server and Nano Server information</span></span>
* <span data-ttu-id="bbedf-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service-innehåll](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="bbedf-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service Content](http://aka.ms/AzureContainerService)</span></span>
* <span data-ttu-id="bbedf-147">Fler exempel i arbetet med Docker finns [arbeta med Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) från hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Anslut [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="bbedf-147">For more examples of working with Docker, see [Working with Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) from hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span> <span data-ttu-id="bbedf-148">Läs mer Snabbstart från hello HealthClinic.biz demo [Azure Developer Tools Snabbstart](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span><span class="sxs-lookup"><span data-stu-id="bbedf-148">For more quickstarts from hello HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>

## <a name="various-docker-tools"></a><span data-ttu-id="bbedf-149">Olika Docker-verktyg</span><span class="sxs-lookup"><span data-stu-id="bbedf-149">Various Docker tools</span></span>
[<span data-ttu-id="bbedf-150">Vissa bra docker-verktyg (Steve Lasker blogg)</span><span class="sxs-lookup"><span data-stu-id="bbedf-150">Some great docker tools (Steve Lasker's blog)</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a><span data-ttu-id="bbedf-151">Bra artiklar</span><span class="sxs-lookup"><span data-stu-id="bbedf-151">Good articles</span></span>
[<span data-ttu-id="bbedf-152">Introduktion tooMicroservices från NGINX</span><span class="sxs-lookup"><span data-stu-id="bbedf-152">Introduction tooMicroservices from NGINX</span></span>](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a><span data-ttu-id="bbedf-153">Presentationer</span><span class="sxs-lookup"><span data-stu-id="bbedf-153">Presentations</span></span>
* [<span data-ttu-id="bbedf-154">Steve Lasker: VS Live nu är det jul 2016 - Docker e2e</span><span class="sxs-lookup"><span data-stu-id="bbedf-154">Steve Lasker: VS Live Las Vegas 2016 - Docker e2e</span></span>](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [<span data-ttu-id="bbedf-155">Introduktion tooASP.NET Core @ build 2016 - där du vid Demo</span><span class="sxs-lookup"><span data-stu-id="bbedf-155">Introduction tooASP.NET Core @ build 2016 - Where You At Demo</span></span>](https://channel9.msdn.com/Events/Build/2016/B810)
* [<span data-ttu-id="bbedf-156">Utveckla .NET appar i behållare, Channel 9</span><span class="sxs-lookup"><span data-stu-id="bbedf-156">Developing .NET apps in containers, Channel 9</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
