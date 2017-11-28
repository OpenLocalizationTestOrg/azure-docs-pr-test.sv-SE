---
title: aaaCreate en ASP.NET Core webbapp i Visual Studio Code
description: "Den här kursen visar hur toocreate en ASP.NET Core web app med Visual Studio-koden."
services: app-service\web
documentationcenter: .net
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 877bff08-9ef7-405a-a1ca-1194f33c55f2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: cephalin
ms.openlocfilehash: 1c18c94984d71e88d2a5b792d68cb1c81e4a96d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a><span data-ttu-id="335b1-103">Skapa en ASP.NET Core webbapp i Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="335b1-103">Create an ASP.NET Core web app in Visual Studio Code</span></span>
## <a name="overview"></a><span data-ttu-id="335b1-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="335b1-104">Overview</span></span>
<span data-ttu-id="335b1-105">Den här kursen visar hur toocreate en ASP.NET Core web app med hjälp av [Visual Studio-koden (VS)](http://code.visualstudio.com//Docs/whyvscode) och distribuera det för[Azure App Service](../app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="335b1-105">This tutorial shows you how toocreate an ASP.NET Core web app using [Visual Studio Code (VS Code)](http://code.visualstudio.com//Docs/whyvscode) and deploy it too[Azure App Service](../app-service/app-service-value-prop-what-is.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="335b1-106">Även om den här artikeln handlar tooweb appar, gäller även tooAPI apps och mobilappar.</span><span class="sxs-lookup"><span data-stu-id="335b1-106">Although this article refers tooweb apps, it also applies tooAPI apps and mobile apps.</span></span> 
> 
> 

<span data-ttu-id="335b1-107">ASP.NET Core är en helt ny utformning av ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="335b1-107">ASP.NET Core is a significant redesign of ASP.NET.</span></span> <span data-ttu-id="335b1-108">ASP.NET Core är ett nytt öppen källkod och plattformsoberoende ramverk för att bygga moderna molnbaserade webbprogram med hjälp av .NET.</span><span class="sxs-lookup"><span data-stu-id="335b1-108">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based web apps using .NET.</span></span> <span data-ttu-id="335b1-109">Mer information finns i [introduktion tooASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span><span class="sxs-lookup"><span data-stu-id="335b1-109">For more information, see [Introduction tooASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span></span> <span data-ttu-id="335b1-110">Information om Azure App Service web apps finns [översikt över Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="335b1-110">For information about Azure App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span>

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a><span data-ttu-id="335b1-111">Krav</span><span class="sxs-lookup"><span data-stu-id="335b1-111">Prerequisites</span></span>
* <span data-ttu-id="335b1-112">Installera [VS koden](http://code.visualstudio.com/Docs/setup).</span><span class="sxs-lookup"><span data-stu-id="335b1-112">Install [VS Code](http://code.visualstudio.com/Docs/setup).</span></span>
* <span data-ttu-id="335b1-113">Installera Git - du kan installera den från någon av dessa platser: [Chocolatey](https://chocolatey.org/packages/git) eller [git scm.com](http://git-scm.com/downloads). Om du är ny tooGit Välj [git scm.com](http://git-scm.com/downloads) och välj alternativet för hello för**använda Git från hello Windows kommandotolk**.</span><span class="sxs-lookup"><span data-stu-id="335b1-113">Install Git - You can install it from either of these locations: [Chocolatey](https://chocolatey.org/packages/git) or [git-scm.com](http://git-scm.com/downloads). If you are new tooGit, choose [git-scm.com](http://git-scm.com/downloads) and select hello option too**Use Git from hello Windows Command Prompt**.</span></span> <span data-ttu-id="335b1-114">När du installerar Git måste du också tooset hello Git-användarnamn och e-post eftersom det krävs senare i självstudiekursen hello (när du utför ett genomförande från VS-kod).</span><span class="sxs-lookup"><span data-stu-id="335b1-114">Once you install Git, you'll also need tooset hello Git user name and email as it's required later in hello tutorial (when performing a commit from VS Code).</span></span>  

## <a name="install-aspnet-core"></a><span data-ttu-id="335b1-115">Installera ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="335b1-115">Install ASP.NET Core</span></span>
<span data-ttu-id="335b1-116">ASP.NET Core är en lean .NET-stack för att bygga moderna molnet och webb-appar som körs på OS X, Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="335b1-116">ASP.NET Core is a lean .NET stack for building modern cloud and web apps that run on OS X, Linux, and Windows.</span></span> <span data-ttu-id="335b1-117">Den byggts från hello bakgrund in tooprovide en optimerad utvecklingsramverk för appar som är antingen distribuerade toohello moln eller kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="335b1-117">It has been built from hello ground up tooprovide an optimized development framework for apps that are either deployed toohello cloud or run on-premises.</span></span> <span data-ttu-id="335b1-118">Det består av modulära komponenter med minimal kostnader, så att du behåller flexibilitet när dina lösningar.</span><span class="sxs-lookup"><span data-stu-id="335b1-118">It consists of modular components with minimal overhead, so you retain flexibility while constructing your solutions.</span></span>

<span data-ttu-id="335b1-119">Den här kursen är utformad tooget som du startade utveckling av program med hello senaste utveckling versioner av ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="335b1-119">This tutorial is designed tooget you started building applications with hello latest development versions of ASP.NET Core.</span></span> <span data-ttu-id="335b1-120">hello följande instruktioner är specifika tooWindows.</span><span class="sxs-lookup"><span data-stu-id="335b1-120">hello following instructions are specific tooWindows.</span></span> <span data-ttu-id="335b1-121">Installationsinstruktioner för OS X, Linux och Windows, se [komma igång med ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span><span class="sxs-lookup"><span data-stu-id="335b1-121">For installation instructions on OS X, Linux, and Windows, see [Getting Started with ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span></span> 


> [!NOTE]
> <span data-ttu-id="335b1-122">Mer detaljerad Installationsinstruktioner för OS X, Linux och Windows finns [installera ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span><span class="sxs-lookup"><span data-stu-id="335b1-122">For more detailed installation instructions for OS X, Linux, and Windows, see [Installing ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span></span> 
> 
> 

## <a name="create-hello-web-app"></a><span data-ttu-id="335b1-123">Skapa hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="335b1-123">Create hello web app</span></span>
<span data-ttu-id="335b1-124">Det här avsnittet visar hur tooscaffold en ny app ASP.NET web app med hello .NET CLI-verktyget.</span><span class="sxs-lookup"><span data-stu-id="335b1-124">This section shows you how tooscaffold a new app ASP.NET web app using hello .NET CLI tool.</span></span> 

1. <span data-ttu-id="335b1-125">Ange följande hello i hello kommandotolk toocreate hello projektet mappen och kodskelett hello app.</span><span class="sxs-lookup"><span data-stu-id="335b1-125">Enter hello following at hello command prompt toocreate hello project folder and scaffold hello app.</span></span>
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![DotNet CLI - ASP.NET Core generator](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. <span data-ttu-id="335b1-127">toorestore Hej nödvändiga NuGet-paket, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="335b1-127">toorestore hello necessary NuGet packages, run hello following command:</span></span>
   
    ```terminal
    dotnet restore
    ```

## <a name="run-hello-web-app-locally"></a><span data-ttu-id="335b1-128">Kör hello webbappen lokalt</span><span class="sxs-lookup"><span data-stu-id="335b1-128">Run hello web app locally</span></span>
<span data-ttu-id="335b1-129">Nu när du har skapat hello webbapp och hämta alla hello NuGet-paket för hello app, kan du köra hello webbappen lokalt.</span><span class="sxs-lookup"><span data-stu-id="335b1-129">Now that you have created hello web app and retrieved all hello NuGet packages for hello app, you can run hello web app locally.</span></span>

1. <span data-ttu-id="335b1-130">Kör hello app (hello `dotnet run` kommando skapar hello app när den är för gammal):</span><span class="sxs-lookup"><span data-stu-id="335b1-130">Run hello app  (hello `dotnet run` command will build hello app when it's out of date):</span></span>
    ```terminal
    dotnet run
    ```
2. <span data-ttu-id="335b1-131">Öppna en webbläsare och gå toohello följande URL: en.</span><span class="sxs-lookup"><span data-stu-id="335b1-131">Open a browser and navigate toohello following URL.</span></span>
   
    <span data-ttu-id="335b1-132">**http://localhost:5000**</span><span class="sxs-lookup"><span data-stu-id="335b1-132">**http://localhost:5000**</span></span>
   
    <span data-ttu-id="335b1-133">Så här visas hello standardsida av hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="335b1-133">hello default page of hello web app will appear as follows.</span></span>
   
    ![Lokala webbprogram i en webbläsare](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. <span data-ttu-id="335b1-135">Stäng webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="335b1-135">Close your browser.</span></span> <span data-ttu-id="335b1-136">I hello **kommandofönstret**, tryck på **Ctrl + C** tooshut hello programmet och Stäng hello **kommandofönstret**.</span><span class="sxs-lookup"><span data-stu-id="335b1-136">In hello **Command Window**, press **Ctrl+C** tooshut down hello application and close hello **Command Window**.</span></span> 

## <a name="create-a-web-app-in-hello-azure-portal"></a><span data-ttu-id="335b1-137">Skapa en webbapp i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="335b1-137">Create a web app in hello Azure Portal</span></span>
<span data-ttu-id="335b1-138">hello hjälper följande steg dig att skapa en webbapp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="335b1-138">hello following steps will guide you through creating a web app in hello Azure Portal.</span></span>

1. <span data-ttu-id="335b1-139">Logga in toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="335b1-139">Log in toohello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="335b1-140">Klicka på **ny** på hello upp till vänster i hello Portal.</span><span class="sxs-lookup"><span data-stu-id="335b1-140">Click **NEW** at hello top left of hello Portal.</span></span>
3. <span data-ttu-id="335b1-141">Klicka på **Web Apps > Webbapp**.</span><span class="sxs-lookup"><span data-stu-id="335b1-141">Click **Web Apps > Web App**.</span></span>
   
    ![Nytt webbprogram för Azure](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. <span data-ttu-id="335b1-143">Ange ett värde för **namn**, som **SampleWebAppDemo**.</span><span class="sxs-lookup"><span data-stu-id="335b1-143">Enter a value for **Name**, such as **SampleWebAppDemo**.</span></span> <span data-ttu-id="335b1-144">Observera att det här namnet måste toobe unika och hello portal kommer att verkställa som när du försöker tooenter hello namn.</span><span class="sxs-lookup"><span data-stu-id="335b1-144">Note that this name needs toobe unique, and hello portal will enforce that when you attempt tooenter hello name.</span></span> <span data-ttu-id="335b1-145">Därför, om du väljer att ange ett annat värde måste toosubstitute som värde för varje förekomst av **SampleWebAppDemo** som visas i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="335b1-145">Therefore, if you select a enter a different value, you'll need toosubstitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span> 
5. <span data-ttu-id="335b1-146">Välj en befintlig **Apptjänstplan** eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="335b1-146">Select an existing **App Service Plan** or create a new one.</span></span> <span data-ttu-id="335b1-147">Om du skapar en ny plan, Välj hello prisnivå, plats, och andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="335b1-147">If you create a new plan, select hello pricing tier, location, and other options.</span></span> <span data-ttu-id="335b1-148">Mer information om apptjänstplaner finns hello artikeln [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="335b1-148">For more information on App Service plans, see hello article, [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
   
    ![Azure web app-bladet](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. <span data-ttu-id="335b1-150">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="335b1-150">Click **Create**.</span></span>
   
    ![Web app bladet](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-hello-new-web-app"></a><span data-ttu-id="335b1-152">Aktivera Git-publicering för hello ny webbapp</span><span class="sxs-lookup"><span data-stu-id="335b1-152">Enable Git publishing for hello new web app</span></span>
<span data-ttu-id="335b1-153">Git är ett kontrollsystem för distribuerade versioner som du kan använda toodeploy din webbapp i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="335b1-153">Git is a distributed version control system that you can use toodeploy your Azure App Service web app.</span></span> <span data-ttu-id="335b1-154">Du lagrar hello koden du skriver för webbappen i en lokal Git-lagringsplats och du kommer att distribuera din kod tooAzure genom att trycka på tooa fjärrdatabasen.</span><span class="sxs-lookup"><span data-stu-id="335b1-154">You'll store hello code you write for your web app in a local Git repository, and you'll deploy your code tooAzure by pushing tooa remote repository.</span></span>   

1. <span data-ttu-id="335b1-155">Logga in på hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="335b1-155">Log into hello [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="335b1-156">Klicka på **Browse** (Bläddra).</span><span class="sxs-lookup"><span data-stu-id="335b1-156">Click **Browse**.</span></span>
3. <span data-ttu-id="335b1-157">Klicka på **Web Apps** tooview en lista över hello webbprogram som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="335b1-157">Click **Web Apps** tooview a list of hello web apps associated with your Azure subscription.</span></span>
4. <span data-ttu-id="335b1-158">Välj hello webbprogram som du skapade i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="335b1-158">Select hello web app you created in this tutorial.</span></span>
5. <span data-ttu-id="335b1-159">Hello web app-bladet, klickar du på **inställningar** > **kontinuerlig distribution**.</span><span class="sxs-lookup"><span data-stu-id="335b1-159">In hello web app blade, click **Settings** > **Continuous deployment**.</span></span> 
   
    ![Azure web app värden](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. <span data-ttu-id="335b1-161">Klicka på **Välj källa > lokal Git-lagringsplats**.</span><span class="sxs-lookup"><span data-stu-id="335b1-161">Click **Choose Source > Local Git Repository**.</span></span>
7. <span data-ttu-id="335b1-162">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="335b1-162">Click **OK**.</span></span>
   
    ![Azure lokal Git-lagringsplatsen](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. <span data-ttu-id="335b1-164">Om du inte tidigare registrerat autentiseringsuppgifter för distribution för att publicera en webbapp eller andra Apptjänst-app, skapa dem nu:</span><span class="sxs-lookup"><span data-stu-id="335b1-164">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>
   
   * <span data-ttu-id="335b1-165">Klicka på **inställningar** > **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="335b1-165">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="335b1-166">Hej **ange autentiseringsuppgifter för distribution** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="335b1-166">hello **Set deployment credentials** blade will be displayed.</span></span>
   * <span data-ttu-id="335b1-167">Skapa ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="335b1-167">Create a user name and password.</span></span>  <span data-ttu-id="335b1-168">Du behöver lösenordet senare när du ställer in Git.</span><span class="sxs-lookup"><span data-stu-id="335b1-168">You'll need this password later when setting up Git.</span></span>
   * <span data-ttu-id="335b1-169">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="335b1-169">Click **Save**.</span></span>
9. <span data-ttu-id="335b1-170">I din webbapps blad klickar du på **Inställningar > Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="335b1-170">In your web app's blade, click **Settings > Properties**.</span></span> <span data-ttu-id="335b1-171">hello-URL för hello fjärransluten Git-lagringsplats som du ska distribuera toois visas under **URL för GIT**.</span><span class="sxs-lookup"><span data-stu-id="335b1-171">hello URL of hello remote Git repository that you'll deploy toois shown under **GIT URL**.</span></span>
10. <span data-ttu-id="335b1-172">Kopiera hello **URL för GIT** värde för senare användning i hello självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="335b1-172">Copy hello **GIT URL** value for later use in hello tutorial.</span></span>
    
    ![Azure Git-URL](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-tooazure-app-service"></a><span data-ttu-id="335b1-174">Publicera din web app tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="335b1-174">Publish your web app tooAzure App Service</span></span>
<span data-ttu-id="335b1-175">I det här avsnittet skapar en lokal Git-lagringsplats och skicka från den databasen tooAzure toodeploy tooAzure dina webb-app.</span><span class="sxs-lookup"><span data-stu-id="335b1-175">In this section, you will create a local Git repository and push from that repository tooAzure toodeploy your web app tooAzure.</span></span>

1. <span data-ttu-id="335b1-176">Välj hello i VS-kod, **Git** alternativet i hello vänstra navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="335b1-176">In VS Code, select hello **Git** option in hello left navigation bar.</span></span>
   
    ![Git-ikonen i VS-kod](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. <span data-ttu-id="335b1-178">Välj **initiera git-lagringsplats** toomake till din arbetsyta källkontroll används git.</span><span class="sxs-lookup"><span data-stu-id="335b1-178">Select **Initialize git repository** toomake sure your workspace is under git source control.</span></span> 
   
    ![Starta Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. <span data-ttu-id="335b1-180">Öppna hello-kommandofönster och ändra kataloger toohello katalogen för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="335b1-180">Open hello Command Window and change directories toohello directory of your web app.</span></span> <span data-ttu-id="335b1-181">Ange sedan hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="335b1-181">Then, enter hello following command:</span></span>
   
        git config core.autocrlf false
   
    <span data-ttu-id="335b1-182">Det här kommandot förhindrar att ett problem om text där CRLF ändelser och LF ändelser ingår.</span><span class="sxs-lookup"><span data-stu-id="335b1-182">This command prevents an issue about text where CRLF endings and LF endings are involved.</span></span>
4. <span data-ttu-id="335b1-183">Lägga till ett genomförande meddelande i VS-kod, och klicka på hello **Commit alla** kryssikon.</span><span class="sxs-lookup"><span data-stu-id="335b1-183">In VS Code, add a commit message and click hello **Commit All** check icon.</span></span>
   
    ![Git genomför alla](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. <span data-ttu-id="335b1-185">När Git har bearbetat ser du att det inte finns några filer som anges i hello Git fönstret under **ändringar**.</span><span class="sxs-lookup"><span data-stu-id="335b1-185">After Git has completed processing, you'll see that there are no files listed in hello Git window under **Changes**.</span></span> 
   
    ![Git inga ändringar](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. <span data-ttu-id="335b1-187">Ändra tillbaka toohello kommandofönster där hello kommandotolk pekar toohello katalog där ditt webbprogram finns.</span><span class="sxs-lookup"><span data-stu-id="335b1-187">Change back toohello Command Window where hello command prompt points toohello directory where your web app is located.</span></span>
7. <span data-ttu-id="335b1-188">Skapa en fjärransluten referens för push-överföra uppdateringar tooyour webbprogram med hjälp av hello Git-URL (slutar med ”.git”) som du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="335b1-188">Create a remote reference for pushing updates tooyour web app by using hello Git URL (ending in ".git") that you copied earlier.</span></span>
   
        git remote add azure [URL for remote repository]
8. <span data-ttu-id="335b1-189">Konfigurera Git toosave dina autentiseringsuppgifter lokalt så att de blir automatiskt tillagda tooyour push-kommandon som genereras från VS-kod.</span><span class="sxs-lookup"><span data-stu-id="335b1-189">Configure Git toosave your credentials locally so that they will be automatically appended tooyour push commands generated from VS Code.</span></span>
   
        git config credential.helper store
9. <span data-ttu-id="335b1-190">Skicka ändringar-tooAzure genom att ange hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="335b1-190">Push your changes tooAzure by entering hello following command.</span></span> <span data-ttu-id="335b1-191">Efter den här första push-tooAzure kommer du att kunna toodo alla hello push-kommandon från VS-kod.</span><span class="sxs-lookup"><span data-stu-id="335b1-191">After this initial push tooAzure, you will be able toodo all hello push commands from VS Code.</span></span> 
   
        git push -u azure master
   
    <span data-ttu-id="335b1-192">Du ombeds hello-lösenord som du skapade tidigare i Azure.</span><span class="sxs-lookup"><span data-stu-id="335b1-192">You are prompted for hello password you created earlier in Azure.</span></span> <span data-ttu-id="335b1-193">**Obs: Lösenordet visas inte.**</span><span class="sxs-lookup"><span data-stu-id="335b1-193">**Note: Your password will not be visible.**</span></span>
   
    <span data-ttu-id="335b1-194">hello utdata från hello ovan kommandot slutar med ett meddelande om att distributionen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="335b1-194">hello output from hello above command ends with a message that deployment is successful.</span></span>
   
        remote: Deployment successful.
        toohttps://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> <span data-ttu-id="335b1-195">Om du gör ändringar tooyour app, kan du publicera direkt i VS kod med hello inbyggda funktioner för Git genom att välja hello **genomför alla** alternativet följt av hello **Push** alternativet.</span><span class="sxs-lookup"><span data-stu-id="335b1-195">If you make changes tooyour app, you can republish directly in VS Code using hello built-in Git functionality by selecting hello **Commit All** option followed by hello **Push** option.</span></span> <span data-ttu-id="335b1-196">Du hittar hello **Push** alternativ som finns i hello nedrullningsbara menyn nästa toohello **genomför alla** och **uppdatera** knappar.</span><span class="sxs-lookup"><span data-stu-id="335b1-196">You will find hello **Push** option available in hello drop-down menu next toohello **Commit All** and **Refresh** buttons.</span></span>
> 
> 

<span data-ttu-id="335b1-197">Om du behöver toocollaborate med ett projekt, bör du överföra tooGitHub between trycka tooAzure.</span><span class="sxs-lookup"><span data-stu-id="335b1-197">If you need toocollaborate on a project, you should consider pushing tooGitHub in between pushing tooAzure.</span></span>

## <a name="run-hello-app-in-azure"></a><span data-ttu-id="335b1-198">Kör hello app i Azure</span><span class="sxs-lookup"><span data-stu-id="335b1-198">Run hello app in Azure</span></span>
<span data-ttu-id="335b1-199">Nu när du har distribuerat ditt webbprogram kör vi hello app medan finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="335b1-199">Now that you have deployed your web app, let's run hello app while hosted in Azure.</span></span> 

<span data-ttu-id="335b1-200">Detta kan göras på två sätt:</span><span class="sxs-lookup"><span data-stu-id="335b1-200">This can be done in two ways:</span></span>

* <span data-ttu-id="335b1-201">Öppna en webbläsare och ange hello namnet på ditt webbprogram på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="335b1-201">Open a browser and enter hello name of your web app as follows.</span></span>   
  
        http://SampleWebAppDemo.azurewebsites.net
* <span data-ttu-id="335b1-202">I hello Azure-portalen, leta upp hello web app bladet för din webbapp och på **Bläddra** tooview din app</span><span class="sxs-lookup"><span data-stu-id="335b1-202">In hello Azure Portal, locate hello web app blade for your web app, and click **Browse** tooview your app</span></span> 
* <span data-ttu-id="335b1-203">i din standardwebbläsare.</span><span class="sxs-lookup"><span data-stu-id="335b1-203">in your default browser.</span></span>

![Azure-webbapp](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a><span data-ttu-id="335b1-205">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="335b1-205">Summary</span></span>
<span data-ttu-id="335b1-206">I kursen får du lärt dig hur toocreate en webbapp i VS-kod och distribuerar den tooAzure.</span><span class="sxs-lookup"><span data-stu-id="335b1-206">In this tutorial, you learned how toocreate a web app in VS Code and deploy it tooAzure.</span></span> <span data-ttu-id="335b1-207">Mer information om VS koden finns hello artikeln [varför Visual Studio-koden?](https://code.visualstudio.com/Docs/)</span><span class="sxs-lookup"><span data-stu-id="335b1-207">For more information about VS Code, see hello article, [Why Visual Studio Code?](https://code.visualstudio.com/Docs/)</span></span> <span data-ttu-id="335b1-208">Information om App Service web apps finns [översikt över Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="335b1-208">For information about App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span> 

