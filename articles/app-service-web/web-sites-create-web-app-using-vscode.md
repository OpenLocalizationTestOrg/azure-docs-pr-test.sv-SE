---
title: Skapa en ASP.NET Core webbapp i Visual Studio Code
description: "Den här kursen visar hur du kan skapa en ASP.NET Core webbprogram med hjälp av Visual Studio-koden."
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
ms.openlocfilehash: 46e3852dc84265de41bb358f482dec06608e7efa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a><span data-ttu-id="41363-103">Skapa en ASP.NET Core webbapp i Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="41363-103">Create an ASP.NET Core web app in Visual Studio Code</span></span>
## <a name="overview"></a><span data-ttu-id="41363-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="41363-104">Overview</span></span>
<span data-ttu-id="41363-105">Den här kursen visar hur du skapar en ASP.NET Core web app med hjälp av [Visual Studio-koden (VS)](http://code.visualstudio.com//Docs/whyvscode) och distribuerar det till [Azure App Service](../app-service/app-service-value-prop-what-is.md).</span><span class="sxs-lookup"><span data-stu-id="41363-105">This tutorial shows you how to create an ASP.NET Core web app using [Visual Studio Code (VS Code)](http://code.visualstudio.com//Docs/whyvscode) and deploy it to [Azure App Service](../app-service/app-service-value-prop-what-is.md).</span></span> 

> [!NOTE]
> <span data-ttu-id="41363-106">Även om den här artikeln handlar om webbappar, så gäller den även för API-appar och mobilappar.</span><span class="sxs-lookup"><span data-stu-id="41363-106">Although this article refers to web apps, it also applies to API apps and mobile apps.</span></span> 
> 
> 

<span data-ttu-id="41363-107">ASP.NET Core är en helt ny utformning av ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="41363-107">ASP.NET Core is a significant redesign of ASP.NET.</span></span> <span data-ttu-id="41363-108">ASP.NET Core är ett nytt öppen källkod och plattformsoberoende ramverk för att bygga moderna molnbaserade webbprogram med hjälp av .NET.</span><span class="sxs-lookup"><span data-stu-id="41363-108">ASP.NET Core is a new open-source and cross-platform framework for building modern cloud-based web apps using .NET.</span></span> <span data-ttu-id="41363-109">Mer information finns i [introduktion till ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span><span class="sxs-lookup"><span data-stu-id="41363-109">For more information, see [Introduction to ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html).</span></span> <span data-ttu-id="41363-110">Information om Azure App Service web apps finns [översikt över Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="41363-110">For information about Azure App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span>

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a><span data-ttu-id="41363-111">Krav</span><span class="sxs-lookup"><span data-stu-id="41363-111">Prerequisites</span></span>
* <span data-ttu-id="41363-112">Installera [VS koden](http://code.visualstudio.com/Docs/setup).</span><span class="sxs-lookup"><span data-stu-id="41363-112">Install [VS Code](http://code.visualstudio.com/Docs/setup).</span></span>
* <span data-ttu-id="41363-113">Installera Git - du kan installera den från någon av dessa platser: [Chocolatey](https://chocolatey.org/packages/git) eller [git scm.com](http://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="41363-113">Install Git - You can install it from either of these locations: [Chocolatey](https://chocolatey.org/packages/git) or [git-scm.com](http://git-scm.com/downloads).</span></span> <span data-ttu-id="41363-114">Om du är nybörjare på Git väljer [git scm.com](http://git-scm.com/downloads) och väljer alternativet att **använda Git från Kommandotolken i Windows**.</span><span class="sxs-lookup"><span data-stu-id="41363-114">If you are new to Git, choose [git-scm.com](http://git-scm.com/downloads) and select the option to **Use Git from the Windows Command Prompt**.</span></span> <span data-ttu-id="41363-115">När du installerar Git, behöver du också ange Git-användarnamn och e-eftersom det krävs senare under kursen (när du utför ett genomförande från VS-kod).</span><span class="sxs-lookup"><span data-stu-id="41363-115">Once you install Git, you'll also need to set the Git user name and email as it's required later in the tutorial (when performing a commit from VS Code).</span></span>  

## <a name="install-aspnet-core"></a><span data-ttu-id="41363-116">Installera ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41363-116">Install ASP.NET Core</span></span>
<span data-ttu-id="41363-117">ASP.NET Core är en lean .NET-stack för att bygga moderna molnet och webb-appar som körs på OS X, Linux och Windows.</span><span class="sxs-lookup"><span data-stu-id="41363-117">ASP.NET Core is a lean .NET stack for building modern cloud and web apps that run on OS X, Linux, and Windows.</span></span> <span data-ttu-id="41363-118">Det har skapats från grunden ge en optimerad utvecklingsramverk för appar som distribuerats till molnet eller kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="41363-118">It has been built from the ground up to provide an optimized development framework for apps that are either deployed to the cloud or run on-premises.</span></span> <span data-ttu-id="41363-119">Det består av modulära komponenter med minimal kostnader, så att du behåller flexibilitet när dina lösningar.</span><span class="sxs-lookup"><span data-stu-id="41363-119">It consists of modular components with minimal overhead, so you retain flexibility while constructing your solutions.</span></span>

<span data-ttu-id="41363-120">Den här kursen är avsedd att komma igång utveckling av program med de senaste utvecklingen versionerna av ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="41363-120">This tutorial is designed to get you started building applications with the latest development versions of ASP.NET Core.</span></span> <span data-ttu-id="41363-121">Följande instruktioner är specifika för Windows.</span><span class="sxs-lookup"><span data-stu-id="41363-121">The following instructions are specific to Windows.</span></span> <span data-ttu-id="41363-122">Installationsinstruktioner för OS X, Linux och Windows, se [komma igång med ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span><span class="sxs-lookup"><span data-stu-id="41363-122">For installation instructions on OS X, Linux, and Windows, see [Getting Started with ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started).</span></span> 


> [!NOTE]
> <span data-ttu-id="41363-123">Mer detaljerad Installationsinstruktioner för OS X, Linux och Windows finns [installera ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span><span class="sxs-lookup"><span data-stu-id="41363-123">For more detailed installation instructions for OS X, Linux, and Windows, see [Installing ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx).</span></span> 
> 
> 

## <a name="create-the-web-app"></a><span data-ttu-id="41363-124">Skapa webbappen</span><span class="sxs-lookup"><span data-stu-id="41363-124">Create the web app</span></span>
<span data-ttu-id="41363-125">Det här avsnittet visar hur du Autogenerera en ny app ASP.NET webbapp med hjälp av verktyget .NET CLI.</span><span class="sxs-lookup"><span data-stu-id="41363-125">This section shows you how to scaffold a new app ASP.NET web app using the .NET CLI tool.</span></span> 

1. <span data-ttu-id="41363-126">Skriv följande i Kommandotolken för att skapa projektmappen och Autogenerera appen.</span><span class="sxs-lookup"><span data-stu-id="41363-126">Enter the following at the command prompt to create the project folder and scaffold the app.</span></span>
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![DotNet CLI - ASP.NET Core generator](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. <span data-ttu-id="41363-128">Kör följande kommando för att återställa nödvändiga NuGet-paket:</span><span class="sxs-lookup"><span data-stu-id="41363-128">To restore the necessary NuGet packages, run the following command:</span></span>
   
    ```terminal
    dotnet restore
    ```

## <a name="run-the-web-app-locally"></a><span data-ttu-id="41363-129">Kör webbappen lokalt</span><span class="sxs-lookup"><span data-stu-id="41363-129">Run the web app locally</span></span>
<span data-ttu-id="41363-130">Nu när du har skapat webbappen och hämta alla NuGet-paket för appen, kan du köra webbappen lokalt.</span><span class="sxs-lookup"><span data-stu-id="41363-130">Now that you have created the web app and retrieved all the NuGet packages for the app, you can run the web app locally.</span></span>

1. <span data-ttu-id="41363-131">Kör appen (den `dotnet run` kommando skapar appen när den är för gammal):</span><span class="sxs-lookup"><span data-stu-id="41363-131">Run the app  (the `dotnet run` command will build the app when it's out of date):</span></span>
    ```terminal
    dotnet run
    ```
2. <span data-ttu-id="41363-132">Öppna en webbläsare och gå till följande URL.</span><span class="sxs-lookup"><span data-stu-id="41363-132">Open a browser and navigate to the following URL.</span></span>
   
    <span data-ttu-id="41363-133">**http://localhost:5000**</span><span class="sxs-lookup"><span data-stu-id="41363-133">**http://localhost:5000**</span></span>
   
    <span data-ttu-id="41363-134">Webbprogrammet standardsida visas på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="41363-134">The default page of the web app will appear as follows.</span></span>
   
    ![Lokala webbprogram i en webbläsare](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. <span data-ttu-id="41363-136">Stäng webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="41363-136">Close your browser.</span></span> <span data-ttu-id="41363-137">I den **kommandofönstret**, tryck på **Ctrl + C** att stänga av programmet och Stäng den **kommandofönstret**.</span><span class="sxs-lookup"><span data-stu-id="41363-137">In the **Command Window**, press **Ctrl+C** to shut down the application and close the **Command Window**.</span></span> 

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="41363-138">Skapa en webbapp i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="41363-138">Create a web app in the Azure Portal</span></span>
<span data-ttu-id="41363-139">Följande steg hjälper dig att skapa en webbapp i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="41363-139">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="41363-140">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="41363-140">Log in to the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="41363-141">Klicka på **ny** längst upp till vänster i portalen.</span><span class="sxs-lookup"><span data-stu-id="41363-141">Click **NEW** at the top left of the Portal.</span></span>
3. <span data-ttu-id="41363-142">Klicka på **Web Apps > Webbapp**.</span><span class="sxs-lookup"><span data-stu-id="41363-142">Click **Web Apps > Web App**.</span></span>
   
    ![Nytt webbprogram för Azure](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. <span data-ttu-id="41363-144">Ange ett värde för **namn**, som **SampleWebAppDemo**.</span><span class="sxs-lookup"><span data-stu-id="41363-144">Enter a value for **Name**, such as **SampleWebAppDemo**.</span></span> <span data-ttu-id="41363-145">Observera att det här namnet måste vara unikt och portal fram som när du försöker ange namnet.</span><span class="sxs-lookup"><span data-stu-id="41363-145">Note that this name needs to be unique, and the portal will enforce that when you attempt to enter the name.</span></span> <span data-ttu-id="41363-146">Därför, om du väljer att ange ett annat värde du behöver ersätta värdet för varje förekomst av **SampleWebAppDemo** som visas i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="41363-146">Therefore, if you select a enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span> 
5. <span data-ttu-id="41363-147">Välj en befintlig **Apptjänstplan** eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="41363-147">Select an existing **App Service Plan** or create a new one.</span></span> <span data-ttu-id="41363-148">Om du skapar en ny plan, Välj prisnivån, plats och andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="41363-148">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="41363-149">Mer information om apptjänstplaner finns i artikeln [Azure App Service-planer djupgående översikt över](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="41363-149">For more information on App Service plans, see the article, [Azure App Service plans in-depth overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
   
    ![Azure web app-bladet](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. <span data-ttu-id="41363-151">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="41363-151">Click **Create**.</span></span>
   
    ![Web app bladet](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="41363-153">Aktivera Git-publicering för den nya webbappen</span><span class="sxs-lookup"><span data-stu-id="41363-153">Enable Git publishing for the new web app</span></span>
<span data-ttu-id="41363-154">Git är ett kontrollsystem för distribuerade versioner som du kan använda för att distribuera din webbapp i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="41363-154">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="41363-155">Du lagrar koden du skriver för webbappen på en lokal Git-lagringsplats och du distribuerar koden till Azure genom att push-överföra den till en fjärransluten lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="41363-155">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>   

1. <span data-ttu-id="41363-156">Logga in på den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="41363-156">Log into the [Azure Portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="41363-157">Klicka på **Browse** (Bläddra).</span><span class="sxs-lookup"><span data-stu-id="41363-157">Click **Browse**.</span></span>
3. <span data-ttu-id="41363-158">Klicka på **Web Apps** att visa en lista över webbappar som är associerade med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="41363-158">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>
4. <span data-ttu-id="41363-159">Välj webbprogram som du skapade i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="41363-159">Select the web app you created in this tutorial.</span></span>
5. <span data-ttu-id="41363-160">I bladet webbapp, klickar du på **inställningar** > **kontinuerlig distribution**.</span><span class="sxs-lookup"><span data-stu-id="41363-160">In the web app blade, click **Settings** > **Continuous deployment**.</span></span> 
   
    ![Azure web app värden](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. <span data-ttu-id="41363-162">Klicka på **Välj källa > lokal Git-lagringsplats**.</span><span class="sxs-lookup"><span data-stu-id="41363-162">Click **Choose Source > Local Git Repository**.</span></span>
7. <span data-ttu-id="41363-163">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="41363-163">Click **OK**.</span></span>
   
    ![Azure lokal Git-lagringsplatsen](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. <span data-ttu-id="41363-165">Om du inte tidigare registrerat autentiseringsuppgifter för distribution för att publicera en webbapp eller andra Apptjänst-app, skapa dem nu:</span><span class="sxs-lookup"><span data-stu-id="41363-165">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>
   
   * <span data-ttu-id="41363-166">Klicka på **inställningar** > **distributionsbehörigheterna**.</span><span class="sxs-lookup"><span data-stu-id="41363-166">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="41363-167">Den **ange autentiseringsuppgifter för distribution** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="41363-167">The **Set deployment credentials** blade will be displayed.</span></span>
   * <span data-ttu-id="41363-168">Skapa ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="41363-168">Create a user name and password.</span></span>  <span data-ttu-id="41363-169">Du behöver lösenordet senare när du ställer in Git.</span><span class="sxs-lookup"><span data-stu-id="41363-169">You'll need this password later when setting up Git.</span></span>
   * <span data-ttu-id="41363-170">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="41363-170">Click **Save**.</span></span>
9. <span data-ttu-id="41363-171">I din webbapps blad klickar du på **Inställningar > Egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="41363-171">In your web app's blade, click **Settings > Properties**.</span></span> <span data-ttu-id="41363-172">URL till en fjärransluten Git-lagringsplats som du distribuerar till visas under **URL för GIT**.</span><span class="sxs-lookup"><span data-stu-id="41363-172">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>
10. <span data-ttu-id="41363-173">Kopiera den **URL för GIT** värde för senare användning i självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="41363-173">Copy the **GIT URL** value for later use in the tutorial.</span></span>
    
    ![Azure Git-URL](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="41363-175">Publicera webbappen till Azure App Service</span><span class="sxs-lookup"><span data-stu-id="41363-175">Publish your web app to Azure App Service</span></span>
<span data-ttu-id="41363-176">I det här avsnittet skapas en lokal Git-lagringsplats och push från den lagringsplatsen till Azure för att distribuera webbappen till Azure.</span><span class="sxs-lookup"><span data-stu-id="41363-176">In this section, you will create a local Git repository and push from that repository to Azure to deploy your web app to Azure.</span></span>

1. <span data-ttu-id="41363-177">I VS-kod, väljer du den **Git** alternativ i det vänstra navigeringsfältet.</span><span class="sxs-lookup"><span data-stu-id="41363-177">In VS Code, select the **Git** option in the left navigation bar.</span></span>
   
    ![Git-ikonen i VS-kod](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. <span data-ttu-id="41363-179">Välj **initiera git-lagringsplats** att säkerställa att din arbetsyta är git källkontroll.</span><span class="sxs-lookup"><span data-stu-id="41363-179">Select **Initialize git repository** to make sure your workspace is under git source control.</span></span> 
   
    ![Starta Git](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. <span data-ttu-id="41363-181">Öppna Kommandotolken och ändra sökvägen till katalogen för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="41363-181">Open the Command Window and change directories to the directory of your web app.</span></span> <span data-ttu-id="41363-182">Ange sedan följande kommando:</span><span class="sxs-lookup"><span data-stu-id="41363-182">Then, enter the following command:</span></span>
   
        git config core.autocrlf false
   
    <span data-ttu-id="41363-183">Det här kommandot förhindrar att ett problem om text där CRLF ändelser och LF ändelser ingår.</span><span class="sxs-lookup"><span data-stu-id="41363-183">This command prevents an issue about text where CRLF endings and LF endings are involved.</span></span>
4. <span data-ttu-id="41363-184">Lägga till ett genomförande meddelande i VS-kod, och klicka på den **Commit alla** kryssikon.</span><span class="sxs-lookup"><span data-stu-id="41363-184">In VS Code, add a commit message and click the **Commit All** check icon.</span></span>
   
    ![Git genomför alla](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. <span data-ttu-id="41363-186">När Git har bearbetat ser du att det inte finns några filer som visas i fönstret Git under **ändringar**.</span><span class="sxs-lookup"><span data-stu-id="41363-186">After Git has completed processing, you'll see that there are no files listed in the Git window under **Changes**.</span></span> 
   
    ![Git inga ändringar](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. <span data-ttu-id="41363-188">Ändra tillbaka till kommandofönstret där Kommandotolken pekar på katalogen där ditt webbprogram finns.</span><span class="sxs-lookup"><span data-stu-id="41363-188">Change back to the Command Window where the command prompt points to the directory where your web app is located.</span></span>
7. <span data-ttu-id="41363-189">Skapa en fjärransluten referens för push-överföra uppdateringar till ditt webbprogram med hjälp av Git-URL (slutar med ”.git”) som du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="41363-189">Create a remote reference for pushing updates to your web app by using the Git URL (ending in ".git") that you copied earlier.</span></span>
   
        git remote add azure [URL for remote repository]
8. <span data-ttu-id="41363-190">Konfigurera Git för att spara dina autentiseringsuppgifter lokalt så att de automatiskt läggs till ditt push-kommandon som genereras från VS-kod.</span><span class="sxs-lookup"><span data-stu-id="41363-190">Configure Git to save your credentials locally so that they will be automatically appended to your push commands generated from VS Code.</span></span>
   
        git config credential.helper store
9. <span data-ttu-id="41363-191">Pusha ändringarna till Azure genom att ange följande kommando.</span><span class="sxs-lookup"><span data-stu-id="41363-191">Push your changes to Azure by entering the following command.</span></span> <span data-ttu-id="41363-192">Du kommer att kunna göra push-kommandon från VS kod efter den här första push till Azure.</span><span class="sxs-lookup"><span data-stu-id="41363-192">After this initial push to Azure, you will be able to do all the push commands from VS Code.</span></span> 
   
        git push -u azure master
   
    <span data-ttu-id="41363-193">Du ombeds ange lösenordet du skapade tidigare i Azure.</span><span class="sxs-lookup"><span data-stu-id="41363-193">You are prompted for the password you created earlier in Azure.</span></span> <span data-ttu-id="41363-194">**Obs: Lösenordet visas inte.**</span><span class="sxs-lookup"><span data-stu-id="41363-194">**Note: Your password will not be visible.**</span></span>
   
    <span data-ttu-id="41363-195">Utdata från kommandot ovan slutar med ett meddelande om att distributionen har lyckats.</span><span class="sxs-lookup"><span data-stu-id="41363-195">The output from the above command ends with a message that deployment is successful.</span></span>
   
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> <span data-ttu-id="41363-196">Om du gör ändringar i din app kan du publicera direkt i VS kod med inbyggda funktioner för Git genom att välja den **genomför alla** alternativet följt av den **Push** alternativet.</span><span class="sxs-lookup"><span data-stu-id="41363-196">If you make changes to your app, you can republish directly in VS Code using the built-in Git functionality by selecting the **Commit All** option followed by the **Push** option.</span></span> <span data-ttu-id="41363-197">Du hittar den **Push** alternativ tillgängliga i den nedrullningsbara menyn bredvid den **genomför alla** och **uppdatera** knappar.</span><span class="sxs-lookup"><span data-stu-id="41363-197">You will find the **Push** option available in the drop-down menu next to the **Commit All** and **Refresh** buttons.</span></span>
> 
> 

<span data-ttu-id="41363-198">Om du behöver samarbeta kring ett projekt bör du sänder till GitHub between push-överföring till Azure.</span><span class="sxs-lookup"><span data-stu-id="41363-198">If you need to collaborate on a project, you should consider pushing to GitHub in between pushing to Azure.</span></span>

## <a name="run-the-app-in-azure"></a><span data-ttu-id="41363-199">Kör appen i Azure</span><span class="sxs-lookup"><span data-stu-id="41363-199">Run the app in Azure</span></span>
<span data-ttu-id="41363-200">Nu när du har distribuerat ditt webbprogram kan vi köra appen medan finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="41363-200">Now that you have deployed your web app, let's run the app while hosted in Azure.</span></span> 

<span data-ttu-id="41363-201">Detta kan göras på två sätt:</span><span class="sxs-lookup"><span data-stu-id="41363-201">This can be done in two ways:</span></span>

* <span data-ttu-id="41363-202">Öppna en webbläsare och ange namnet på ditt webbprogram på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="41363-202">Open a browser and enter the name of your web app as follows.</span></span>   
  
        http://SampleWebAppDemo.azurewebsites.net
* <span data-ttu-id="41363-203">Leta upp bladet webbapp för webbappen i Azure-portalen och på **Bläddra** att visa din app</span><span class="sxs-lookup"><span data-stu-id="41363-203">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app</span></span> 
* <span data-ttu-id="41363-204">i din standardwebbläsare.</span><span class="sxs-lookup"><span data-stu-id="41363-204">in your default browser.</span></span>

![Azure-webbapp](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a><span data-ttu-id="41363-206">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="41363-206">Summary</span></span>
<span data-ttu-id="41363-207">I den här självstudiekursen beskrivs hur du skapar en webbapp i VS-kod och distribuerar den till Azure.</span><span class="sxs-lookup"><span data-stu-id="41363-207">In this tutorial, you learned how to create a web app in VS Code and deploy it to Azure.</span></span> <span data-ttu-id="41363-208">Mer information om VS kod finns i artikeln [varför Visual Studio-koden?](https://code.visualstudio.com/Docs/)</span><span class="sxs-lookup"><span data-stu-id="41363-208">For more information about VS Code, see the article, [Why Visual Studio Code?](https://code.visualstudio.com/Docs/)</span></span> <span data-ttu-id="41363-209">Information om App Service web apps finns [översikt över Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="41363-209">For information about App Service web apps, see [Web Apps Overview](app-service-web-overview.md).</span></span> 

