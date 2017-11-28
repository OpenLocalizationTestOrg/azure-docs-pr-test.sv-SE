---
title: "aaaUse .NET Core i webbprogrammet på Linux | Microsoft Docs"
description: "Använd .NET Core i webbprogrammet på Linux."
keywords: "Azure apptjänst, webbprogram, dotnet, core, linux, oss"
services: app-service
documentationCenter: 
authors: michimune, rachelappel
manager: erikre
editor: 
ms.assetid: c02959e6-7220-496a-a417-9b2147638e2e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: aelnably;wesmc;mikono;rachelap
ms.openlocfilehash: 9b7fb7185dff2c99ed88e7937d455177504937b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a><span data-ttu-id="c0cfe-104">Använda .NET Core i en Azure-webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="c0cfe-104">Use .NET Core in an Azure web app on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="c0cfe-105">[Webbapp](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) på Linux innehåller en mycket skalbar, automatisk uppdatering värd webbtjänsten använder hello Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-105">[Web App](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) on Linux provides a highly scalable, self-patching web hosting service using hello Linux operating system.</span></span> <span data-ttu-id="c0cfe-106">Den här självstudiekursen innehåller stegvisa instruktioner som visar hur toocreate en [.NET Core](https://docs.microsoft.com/aspnet/core/) app på Azure-webbapp på Linux.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-106">This tutorial contains step-by-step instructions showing how toocreate a [.NET Core](https://docs.microsoft.com/aspnet/core/) app on Azure web app on Linux.</span></span> 

![Webbapp i Linux][10]

<span data-ttu-id="c0cfe-108">Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c0cfe-109">Krav</span><span class="sxs-lookup"><span data-stu-id="c0cfe-109">Prerequisites</span></span> ##

<span data-ttu-id="c0cfe-110">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-110">toocomplete this tutorial:</span></span> 

* <span data-ttu-id="c0cfe-111">Installera hello [.NET Core SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="c0cfe-111">Install hello [.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>
* <span data-ttu-id="c0cfe-112">Installera [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="c0cfe-112">Install [Git](https://git-scm.com/downloads).</span></span>

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a><span data-ttu-id="c0cfe-113">Skapa ett lokalt program i .NET Core</span><span class="sxs-lookup"><span data-stu-id="c0cfe-113">Create a local .NET Core application</span></span> ##

<span data-ttu-id="c0cfe-114">Starta en ny terminalsession.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-114">Start a new terminal session.</span></span> <span data-ttu-id="c0cfe-115">Skapa en katalog med namnet `hellodotnetcore`, och ändra hello aktuella directory tooit.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-115">Create a directory named `hellodotnetcore`, and change hello current directory tooit.</span></span> <span data-ttu-id="c0cfe-116">Skriv hello följande:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-116">Then type hello following:</span></span> 

```
dotnet new web
``` 

  <span data-ttu-id="c0cfe-117">Det här kommandot skapar tre filer (*hellodotnetcore.csproj*, *Program.cs*, och *Startup.cs*) och en tom mapp (*wwwroot /*) under hello aktuell katalog.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-117">This command creates three files (*hellodotnetcore.csproj*, *Program.cs*, and *Startup.cs*) and one empty folder (*wwwroot/*) under hello current directory.</span></span> <span data-ttu-id="c0cfe-118">innehållet i hello `.csproj` filen bör se ut hello följande:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-118">hello content of `.csproj` file should look like hello following:</span></span> 

```xml
  <!-- Empty lines are omitted. -->

  <Project Sdk="Microsoft.NET.Sdk.Web">
        <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
        <Folder Include="wwwroot\" />
        </ItemGroup>
        <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore" Version="1.1.2" />
        </ItemGroup>
  </Project>
```

<span data-ttu-id="c0cfe-119">Eftersom den här appen är ett webbprogram, läggs en referens tooan ASP.NET Core-paketet har automatiskt toohello *hellodotnetcore.csproj* fil.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-119">Since this app is a web application, a reference tooan ASP.NET Core package was automatically added toohello *hellodotnetcore.csproj* file.</span></span> <span data-ttu-id="c0cfe-120">hello versionsnumret för hello paketet anges enligt toohello valt framework.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-120">hello version number of hello package is set according toohello chosen framework.</span></span> <span data-ttu-id="c0cfe-121">Det här exemplet refererar till ASP.NET Core version 1.1.2 eftersom .NET Core 1.1 används.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-121">This example is referencing ASP.NET Core version 1.1.2 because .NET Core 1.1 is used.</span></span>

## <a name="build-and-test-hello-application-locally"></a><span data-ttu-id="c0cfe-122">Skapa och testa hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="c0cfe-122">Build and test hello application locally</span></span> ##

<span data-ttu-id="c0cfe-123">Du kan skapa och köra appen .NET Core med hello `dotnet restore` kommandot följt av hello `dotnet run` kommandot som visas här:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-123">You can build and run your .NET Core app with hello `dotnet restore` command followed by hello `dotnet run` command, as shown here:</span></span>

```
dotnet restore
dotnet run
```


<span data-ttu-id="c0cfe-124">När programmet hello startar visar ett meddelande om hello app lyssnar tooincoming begäranden på en port.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-124">When hello application starts, it displays a message indicating hello app is listening tooincoming requests at a port.</span></span> 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C tooshut down.
```

<span data-ttu-id="c0cfe-125">Testa den genom att bläddra för`http://localhost:5000/` med din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-125">Test it by browsing too`http://localhost:5000/` with your browser.</span></span> <span data-ttu-id="c0cfe-126">Om allt fungerar bra visas ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="c0cfe-126">If everything works fine, you see "Hello World!"</span></span> <span data-ttu-id="c0cfe-127">som hello resultattext.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-127">as hello result text.</span></span>

![Testa med webbläsare][7]

## <a name="create-a-net-core-app-in-hello-azure-portal"></a><span data-ttu-id="c0cfe-129">Skapa en .NET Core-app i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c0cfe-129">Create a .NET Core app in hello Azure Portal</span></span> ##

<span data-ttu-id="c0cfe-130">Du måste först toocreate ett tomt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-130">First you need toocreate an empty web app.</span></span> <span data-ttu-id="c0cfe-131">Logga in toohello [Azure-portalen](https://portal.azure.com/) och skapa en ny [webbprogrammet på Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span><span class="sxs-lookup"><span data-stu-id="c0cfe-131">Log in toohello [Azure portal](https://portal.azure.com/) and create a new [Web App on Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span></span>

![Skapa en webbapp][1]

<span data-ttu-id="c0cfe-133">När hello **skapa** öppnas, innehåller information om ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-133">When hello **Create** page opens, provide details about your web app:</span></span>

![Om du väljer en .NET Core runtime stack][2]

<span data-ttu-id="c0cfe-135">Använd hello följande tabell som en guide toofill ut hello **skapa** sidan och välj sedan **OK** och **skapa** toocreate hello app.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-135">Use hello following table as a guide toofill out hello **Create** page, then select **OK** and **Create** toocreate hello app.</span></span>

| <span data-ttu-id="c0cfe-136">Inställning</span><span class="sxs-lookup"><span data-stu-id="c0cfe-136">Setting</span></span>      | <span data-ttu-id="c0cfe-137">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="c0cfe-137">Suggested value</span></span>  | <span data-ttu-id="c0cfe-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="c0cfe-138">Description</span></span>                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| <span data-ttu-id="c0cfe-139">Appnamn</span><span class="sxs-lookup"><span data-stu-id="c0cfe-139">App name</span></span> | <span data-ttu-id="c0cfe-140">hellodotnetcore</span><span class="sxs-lookup"><span data-stu-id="c0cfe-140">hellodotnetcore</span></span>  | <span data-ttu-id="c0cfe-141">hello namnet på din app.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-141">hello name of your app.</span></span> <span data-ttu-id="c0cfe-142">Det här namnet måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-142">This name must be unique.</span></span> |
| <span data-ttu-id="c0cfe-143">Prenumeration</span><span class="sxs-lookup"><span data-stu-id="c0cfe-143">Subscription</span></span> | <span data-ttu-id="c0cfe-144">Välj en befintlig prenumeration</span><span class="sxs-lookup"><span data-stu-id="c0cfe-144">Choose an existing subscription</span></span> | <span data-ttu-id="c0cfe-145">hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-145">hello Azure subscription.</span></span> |
| <span data-ttu-id="c0cfe-146">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="c0cfe-146">Resource Group</span></span> | <span data-ttu-id="c0cfe-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c0cfe-147">myResourceGroup</span></span> |  <span data-ttu-id="c0cfe-148">hello Azure-resurs grupp toocontain hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-148">hello Azure resource group toocontain hello web app.</span></span> |
| <span data-ttu-id="c0cfe-149">App Service-plan</span><span class="sxs-lookup"><span data-stu-id="c0cfe-149">App Service Plan</span></span> | <span data-ttu-id="c0cfe-150">Namn på befintliga App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="c0cfe-150">Existing App Service Plan name</span></span> |  <span data-ttu-id="c0cfe-151">hello App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-151">hello App Service plan.</span></span>  |
| <span data-ttu-id="c0cfe-152">Konfigurera behållare</span><span class="sxs-lookup"><span data-stu-id="c0cfe-152">Configure Container</span></span> | <span data-ttu-id="c0cfe-153">.NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="c0cfe-153">.NET Core 1.1</span></span> | <span data-ttu-id="c0cfe-154">Hej typ av behållare för det här webbprogrammet: inbyggda, Docker eller privata registret.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-154">hello type of container for this web app: Built-in, Docker, or Private registry.</span></span> |
| <span data-ttu-id="c0cfe-155">Bildens källa</span><span class="sxs-lookup"><span data-stu-id="c0cfe-155">Image source</span></span>  | <span data-ttu-id="c0cfe-156">Inbyggd</span><span class="sxs-lookup"><span data-stu-id="c0cfe-156">Built-in</span></span>  |  <span data-ttu-id="c0cfe-157">hello källa hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-157">hello source of hello image.</span></span> |
| <span data-ttu-id="c0cfe-158">Runtime-stacken</span><span class="sxs-lookup"><span data-stu-id="c0cfe-158">Runtime Stack</span></span>  | <span data-ttu-id="c0cfe-159">.NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="c0cfe-159">.NET Core 1.1</span></span>  | <span data-ttu-id="c0cfe-160">hello runtime-stacken och version.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-160">hello runtime stack and version.</span></span>  |

## <a name="deploy-your-application-via-git"></a><span data-ttu-id="c0cfe-161">Distribuera programmet via Git</span><span class="sxs-lookup"><span data-stu-id="c0cfe-161">Deploy your application via Git</span></span> ##

<span data-ttu-id="c0cfe-162">Använda Git toodeploy hello .NET Core programmet tooAzure App Service Web App på Linux.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-162">Use Git toodeploy hello .NET Core application tooAzure App Service Web App on Linux.</span></span>

<span data-ttu-id="c0cfe-163">hello nya Azure-webbapp har redan konfigurerats Git-distribution.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-163">hello new Azure web app already has Git deployment configured.</span></span> <span data-ttu-id="c0cfe-164">Du hittar hello URL för Git-distribution genom att gå toohello följande URL när du har infogat din webbprogrammets namn:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-164">You will find hello Git deployment URL by navigating toohello following URL after inserting your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

<span data-ttu-id="c0cfe-165">hello URL för Git har hello följande baserat på din webbprogrammets namn:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-165">hello Git URL has hello following form based on your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

<span data-ttu-id="c0cfe-166">Kör följande kommandon toodeploy hello lokalt program tooyour Azure webbapp hello:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-166">Run hello following commands toodeploy hello local application tooyour Azure web app:</span></span> 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="c0cfe-167">Du behöver inte toopush alla filer under *bin /* eller *obj /* kataloger eftersom programmet är inbyggd i molnet hello när hello programmets tooAzure push-källfiler.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-167">You don't need toopush any files under *bin/* or *obj/* directories because your application is built in hello cloud when hello application's source files are pushed tooAzure.</span></span> <span data-ttu-id="c0cfe-168">När hello build-processen är klar, binära filer kopieras till hello tillämpningsprogrammets katalog på */home/plats/wwwroot/*.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-168">After hello build process is complete, binary files are copied into hello application's directory at */home/site/wwwroot/*.</span></span>

<span data-ttu-id="c0cfe-169">Bekräfta att hello remote distributionsåtgärder rapporterar lyckades.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-169">Confirm that hello remote deployment operations report success.</span></span> <span data-ttu-id="c0cfe-170">Push-åtgärder kan ta ett tag sedan paketet upplösning och skapa processen genom att köra i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-170">Push operations may take a while since package resolution and build process run in hello cloud.</span></span> <span data-ttu-id="c0cfe-171">Du ser flera statusmeddelanden, inklusive de som anger att filerna har kopierats.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-171">You will see several status messages, including ones stating that files have been copied.</span></span> <span data-ttu-id="c0cfe-172">hello utdata ska se ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-172">hello output should look similar toohello following:</span></span>

```bash
/* some output has been removed for brevity */
remote: Copying file: 'System.Net.Websockets.dll' 
remote: Copying file: 'System.Runtime.CompilerServices.Unsafe.dll' 
remote: Copying file: 'System.Runtime.Serialization.Primitives.dll' 
remote: Copying file: 'System.Text.Encodings.Web.dll' 
remote: Copying file: 'hellodotnetcore.deps.json' 
remote: Copying file: 'hellodotnetcore.dll' 
remote: Omitting next output lines...
remote: Finished successfully.
remote: Running post deployment commands...
remote: Deployment successful.
toohttps://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

<span data-ttu-id="c0cfe-173">Starta om ditt webbprogram för hello distribution tootake effekt när hello distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-173">Once hello deployment has completed, restart your web app for hello deployment tootake effect.</span></span> <span data-ttu-id="c0cfe-174">toodo kan gå toohello Azure-portalen och navigera toohello **översikt** sidan av ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-174">toodo this, go toohello Azure portal and navigate toohello **Overview** page of your web app.</span></span> <span data-ttu-id="c0cfe-175">Välj hello **starta om** knappen hello på sidan.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-175">Select hello **Restart** button in hello page.</span></span> <span data-ttu-id="c0cfe-176">När ett popup-fönster visas, väljer du **Ja** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="c0cfe-176">When a popup window shows up, select **Yes** tooconfirm.</span></span> <span data-ttu-id="c0cfe-177">Du kan sedan bläddra ditt webbprogram som visas här:</span><span class="sxs-lookup"><span data-stu-id="c0cfe-177">You can then browse your web app, as shown here:</span></span>

![Bläddra .NET Core distribuerats appen tooAzure Apptjänst på Linux][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="c0cfe-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0cfe-179">Next steps</span></span>
* [<span data-ttu-id="c0cfe-180">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="c0cfe-180">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
