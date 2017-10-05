---
title: "Felsökning av appar i en lokal dockerbehållare | Microsoft Docs"
description: "Lär dig hur du ändrar en app som körs i en lokal dockerbehållare, uppdatera behållaren via Redigera och uppdatera och ange brytpunkter-felsökning"
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
ms.openlocfilehash: fcd58736d8915a61683a416fb9bf3892ba7b7bd8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="debugging-apps-in-a-local-docker-container"></a><span data-ttu-id="7dc16-103">Felsök appar i en lokal Docker-behållare</span><span class="sxs-lookup"><span data-stu-id="7dc16-103">Debugging apps in a local Docker container</span></span>
## <a name="overview"></a><span data-ttu-id="7dc16-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7dc16-104">Overview</span></span>
<span data-ttu-id="7dc16-105">Visual Studio Tools för Docker ger ett konsekvent sätt att utveckla i och validera ditt program lokalt i en Linux-dockerbehållare.</span><span class="sxs-lookup"><span data-stu-id="7dc16-105">The Visual Studio Tools for Docker provides a consistent way to develop in and validate your application locally in a Linux Docker container.</span></span>
<span data-ttu-id="7dc16-106">Du behöver inte starta om behållaren varje gång du gör en kod ändra.</span><span class="sxs-lookup"><span data-stu-id="7dc16-106">You don't have to restart the container each time you make a code change.</span></span>
<span data-ttu-id="7dc16-107">Den här artikeln visar hur funktionen ”Redigera och uppdatera” för att starta en ASP.NET Core webbprogram i en lokal dockerbehållare, gör nödvändiga ändringar och sedan uppdatera webbläsaren om du vill se ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7dc16-107">This article illustrates how to use the "Edit and Refresh" feature to start an ASP.NET Core Web app in a local Docker container, make any necessary changes, and then refresh the browser to see those changes.</span></span>
<span data-ttu-id="7dc16-108">Den här artikeln beskriver också hur du ställer in brytpunkter för felsökning.</span><span class="sxs-lookup"><span data-stu-id="7dc16-108">This article also shows you how to set breakpoints for debugging.</span></span>

> [!NOTE]
> <span data-ttu-id="7dc16-109">Stöd för Windows-behållare kommer i en framtida version</span><span class="sxs-lookup"><span data-stu-id="7dc16-109">Windows Container support will be coming in a future release</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="7dc16-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7dc16-110">Prerequisites</span></span>
<span data-ttu-id="7dc16-111">Följande verktyg måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="7dc16-111">The following tools must be installed.</span></span>

* [<span data-ttu-id="7dc16-112">Senaste version av Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7dc16-112">Latest version of Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="7dc16-113">Microsoft ASP.NET Core 1.0 SDK</span><span class="sxs-lookup"><span data-stu-id="7dc16-113">Microsoft ASP.NET Core 1.0 SDK</span></span>](https://go.microsoft.com/fwlink/?LinkID=809122)

<span data-ttu-id="7dc16-114">Om du vill köra Docker behållare lokalt, behöver du en lokal docker-klient.</span><span class="sxs-lookup"><span data-stu-id="7dc16-114">To run Docker containers locally, you'll need a local docker client.</span></span>
<span data-ttu-id="7dc16-115">Du kan använda den [Docker verktygslådan](https://www.docker.com/products/docker-toolbox), vilket kräver Hyper-V kan inaktiveras eller du kan använda [Docker för Windows](https://www.docker.com/get-docker), som använder Hyper-V och kräver att Windows 10.</span><span class="sxs-lookup"><span data-stu-id="7dc16-115">You can use the [Docker Toolbox](https://www.docker.com/products/docker-toolbox), which requires Hyper-V to be disabled, or you can use [Docker for Windows](https://www.docker.com/get-docker), which uses Hyper-V, and requires Windows 10.</span></span>

<span data-ttu-id="7dc16-116">Om du använder Docker verktygslådan, måste du [konfigurerar Docker-klient](vs-azure-tools-docker-setup.md)</span><span class="sxs-lookup"><span data-stu-id="7dc16-116">If using Docker Toolbox, you'll need to [configure the Docker client](vs-azure-tools-docker-setup.md)</span></span>

## <a name="1-create-a-web-app"></a><span data-ttu-id="7dc16-117">1. Skapa en webbapp</span><span class="sxs-lookup"><span data-stu-id="7dc16-117">1. Create a web app</span></span>
[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a><span data-ttu-id="7dc16-118">2. Lägga till Docker-stöd</span><span class="sxs-lookup"><span data-stu-id="7dc16-118">2. Add Docker support</span></span>
[!INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-edit-your-code-and-refresh"></a><span data-ttu-id="7dc16-119">3. Redigera koden och uppdatera</span><span class="sxs-lookup"><span data-stu-id="7dc16-119">3. Edit your code and refresh</span></span>
<span data-ttu-id="7dc16-120">För att snabbt iterera ändringar, kan du starta appen i en behållare och fortsätta att göra ändringar, visa dem precis som med IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7dc16-120">To quickly iterate changes, you can start your application within a container, and continue to make changes, viewing them as you would with IIS Express.</span></span>

1. <span data-ttu-id="7dc16-121">Konfigurera lösningen `Debug` och tryck på  **&lt;CTRL + F5 >** att skapa en docker-avbildning och köra det lokalt.</span><span class="sxs-lookup"><span data-stu-id="7dc16-121">Set the Solution Configuration to `Debug` and press **&lt;CTRL + F5>** to build your docker image and run it locally.</span></span>

    <span data-ttu-id="7dc16-122">När avbildningen behållaren har skapats och körs i en dockerbehållare, startas Visual Studio webbappen i din standardwebbläsare.</span><span class="sxs-lookup"><span data-stu-id="7dc16-122">Once the container image has been built and is running in a Docker container, Visual Studio will launch the Web app in your default browser.</span></span>
    <span data-ttu-id="7dc16-123">Om du använder Microsoft Edge-webbläsaren eller på annat sätt har fel finns [felsökning](vs-azure-tools-docker-troubleshooting-docker-errors.md) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7dc16-123">If you are using the Microsoft Edge browser or otherwise have errors, see [Troubleshooting](vs-azure-tools-docker-troubleshooting-docker-errors.md) section.</span></span>
2. <span data-ttu-id="7dc16-124">Gå till sidan om, vilket är där vi göra våra ändringar.</span><span class="sxs-lookup"><span data-stu-id="7dc16-124">Go to the About page, which is where we're going to make our changes.</span></span>
3. <span data-ttu-id="7dc16-125">Gå tillbaka till Visual Studio och öppna `Views\Home\About.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="7dc16-125">Return to Visual Studio and open `Views\Home\About.cshtml`.</span></span>
4. <span data-ttu-id="7dc16-126">Lägg till följande HTML-innehåll i slutet av filen och spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="7dc16-126">Add the following HTML content to the end of the file and save the changes.</span></span>

    ```
    <h1>Hello from a Docker Container!</h1>
    ```
5. <span data-ttu-id="7dc16-127">Visa utdatafönstret när .NET-versionen har slutförts och du ser dessa rader kan gå tillbaka till din webbläsare och uppdatera sidan om.</span><span class="sxs-lookup"><span data-stu-id="7dc16-127">Viewing the output window, when the .NET build is completed and you see these lines, switch back to your browser and refresh the About page.</span></span>

   ```
   Now listening on: http://*:80
   Application started. Press Ctrl+C to shut down
   ```
6. <span data-ttu-id="7dc16-128">Ändringarna har tillämpats!</span><span class="sxs-lookup"><span data-stu-id="7dc16-128">Your changes have been applied!</span></span>

## <a name="4-debug-with-breakpoints"></a><span data-ttu-id="7dc16-129">4. Felsöka med brytpunkter</span><span class="sxs-lookup"><span data-stu-id="7dc16-129">4. Debug with breakpoints</span></span>
<span data-ttu-id="7dc16-130">Ofta behöver ändringar ytterligare kontroll, utnyttja funktionerna för felsökning av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7dc16-130">Often, changes will need further inspection, leveraging the debugging features of Visual Studio.</span></span>

1. <span data-ttu-id="7dc16-131">Gå tillbaka till Visual Studio och öppna`Controllers\HomeController.cs`</span><span class="sxs-lookup"><span data-stu-id="7dc16-131">Return to Visual Studio and open `Controllers\HomeController.cs`</span></span>
2. <span data-ttu-id="7dc16-132">Ersätt innehållet i metoden About() med följande:</span><span class="sxs-lookup"><span data-stu-id="7dc16-132">Replace the contents of the About() method with the following:</span></span>

   ```
   string message = "Your application description page from within a Container";
   ViewData["Message"] = message;
   ````
3. <span data-ttu-id="7dc16-133">Ange en brytpunkt till vänster om den `string message`... rad.</span><span class="sxs-lookup"><span data-stu-id="7dc16-133">Set a breakpoint to the left of the `string message`... line.</span></span>
4. <span data-ttu-id="7dc16-134">Träffa  **&lt;F5 >** att starta felsökningen.</span><span class="sxs-lookup"><span data-stu-id="7dc16-134">Hit **&lt;F5>** to start debugging.</span></span>
5. <span data-ttu-id="7dc16-135">Gå till sidan om att träffa din brytpunkt.</span><span class="sxs-lookup"><span data-stu-id="7dc16-135">Navigate to the About page to hit your breakpoint.</span></span>
6. <span data-ttu-id="7dc16-136">Växla till Visual Studio för att visa brytpunkten och kontrollera värdet för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="7dc16-136">Switch to Visual Studio to view the breakpoint, and inspect the value of message.</span></span>

   ![][2]

## <a name="summary"></a><span data-ttu-id="7dc16-137">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7dc16-137">Summary</span></span>
<span data-ttu-id="7dc16-138">Med [Visual Studio 2015 Tools för Docker](https://aka.ms/DockerToolsForVS), kan du arbeta lokalt, med produktion mer att utveckla i en dockerbehållare produktivitet.</span><span class="sxs-lookup"><span data-stu-id="7dc16-138">With [Visual Studio 2015 Tools for Docker](https://aka.ms/DockerToolsForVS), you can get the productivity of working locally, with the production realism of developing within a Docker container.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7dc16-139">Felsökning</span><span class="sxs-lookup"><span data-stu-id="7dc16-139">Troubleshooting</span></span>
[<span data-ttu-id="7dc16-140">Felsökning av Visual Studio Docker-utveckling</span><span class="sxs-lookup"><span data-stu-id="7dc16-140">Troubleshooting Visual Studio Docker Development</span></span>](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a><span data-ttu-id="7dc16-141">Mer information om Docker med Visual Studio och Windows Azure</span><span class="sxs-lookup"><span data-stu-id="7dc16-141">More about Docker with Visual Studio, Windows, and Azure</span></span>
* <span data-ttu-id="7dc16-142">[Docker Tools för Visual Studio](http://aka.ms/dockertoolsforvs) -utveckla .NET Core koden i en behållare</span><span class="sxs-lookup"><span data-stu-id="7dc16-142">[Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - Developing your .NET Core code in a container</span></span>
* <span data-ttu-id="7dc16-143">[Docker-verktyg för Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) – skapa och distribuera docker-behållare</span><span class="sxs-lookup"><span data-stu-id="7dc16-143">[Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - Build and Deploy docker containers</span></span>
* <span data-ttu-id="7dc16-144">[Docker-verktyg för Visual Studio Code](http://aka.ms/dockertoolsforvscode) -språk services för att redigera docker-filer, med mer e2e scenarier kommer</span><span class="sxs-lookup"><span data-stu-id="7dc16-144">[Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) - Language services for editing docker files, with more e2e scenarios coming</span></span>
* <span data-ttu-id="7dc16-145">[Information om Windows behållaren](http://aka.ms/containers)-information för Windows Server och Nano Server</span><span class="sxs-lookup"><span data-stu-id="7dc16-145">[Windows Container Information](http://aka.ms/containers)- Windows Server and Nano Server information</span></span>
* <span data-ttu-id="7dc16-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service-innehåll](http://aka.ms/AzureContainerService)</span><span class="sxs-lookup"><span data-stu-id="7dc16-146">[Azure Container Service](https://azure.microsoft.com/services/container-service/) - [Azure Container Service Content](http://aka.ms/AzureContainerService)</span></span>
* <span data-ttu-id="7dc16-147">Fler exempel i arbetet med Docker finns [arbeta med Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) från den [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Anslut [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="7dc16-147">For more examples of working with Docker, see [Working with Docker](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) from the [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span> <span data-ttu-id="7dc16-148">Fler snabbstartsguider från HealthClinic.biz-demonstrationen finns i [Snabbstartsguider för Azure Developer Tools](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span><span class="sxs-lookup"><span data-stu-id="7dc16-148">For more quickstarts from the HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>

## <a name="various-docker-tools"></a><span data-ttu-id="7dc16-149">Olika Docker-verktyg</span><span class="sxs-lookup"><span data-stu-id="7dc16-149">Various Docker tools</span></span>
[<span data-ttu-id="7dc16-150">Vissa bra docker-verktyg (Steve Lasker blogg)</span><span class="sxs-lookup"><span data-stu-id="7dc16-150">Some great docker tools (Steve Lasker's blog)</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a><span data-ttu-id="7dc16-151">Bra artiklar</span><span class="sxs-lookup"><span data-stu-id="7dc16-151">Good articles</span></span>
[<span data-ttu-id="7dc16-152">Introduktion till Mikrotjänster från NGINX</span><span class="sxs-lookup"><span data-stu-id="7dc16-152">Introduction to Microservices from NGINX</span></span>](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a><span data-ttu-id="7dc16-153">Presentationer</span><span class="sxs-lookup"><span data-stu-id="7dc16-153">Presentations</span></span>
* [<span data-ttu-id="7dc16-154">Steve Lasker: VS Live nu är det jul 2016 - Docker e2e</span><span class="sxs-lookup"><span data-stu-id="7dc16-154">Steve Lasker: VS Live Las Vegas 2016 - Docker e2e</span></span>](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
* [<span data-ttu-id="7dc16-155">Introduktion till ASP.NET Core @ build 2016 - där du vid Demo</span><span class="sxs-lookup"><span data-stu-id="7dc16-155">Introduction to ASP.NET Core @ build 2016 - Where You At Demo</span></span>](https://channel9.msdn.com/Events/Build/2016/B810)
* [<span data-ttu-id="7dc16-156">Utveckla .NET appar i behållare, Channel 9</span><span class="sxs-lookup"><span data-stu-id="7dc16-156">Developing .NET apps in containers, Channel 9</span></span>](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
