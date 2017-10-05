---
title: "Använda .NET Core i webbprogrammet på Linux | Microsoft Docs"
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
ms.openlocfilehash: 9226dfb90e52ac2cae2cfc4af7c0705a93f56b44
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-net-core-in-an-azure-web-app-on-linux"></a><span data-ttu-id="df35f-104">Använda .NET Core i en Azure-webbapp på Linux</span><span class="sxs-lookup"><span data-stu-id="df35f-104">Use .NET Core in an Azure web app on Linux</span></span> #

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="df35f-105">[Webbapp](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) på Linux ger en mycket skalbar, automatisk uppdatering värdtjänst med Linux-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="df35f-105">[Web App](https://docs.microsoft.com/azure/app-service-web/app-service-linux-intro) on Linux provides a highly scalable, self-patching web hosting service using the Linux operating system.</span></span> <span data-ttu-id="df35f-106">Den här självstudiekursen innehåller stegvisa instruktioner för hur du skapar en [.NET Core](https://docs.microsoft.com/aspnet/core/) app på Azure-webbapp på Linux.</span><span class="sxs-lookup"><span data-stu-id="df35f-106">This tutorial contains step-by-step instructions showing how to create a [.NET Core](https://docs.microsoft.com/aspnet/core/) app on Azure web app on Linux.</span></span> 

![Webbapp i Linux][10]

<span data-ttu-id="df35f-108">Du kan följa stegen nedan på en Mac-, Windows- eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="df35f-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df35f-109">Krav</span><span class="sxs-lookup"><span data-stu-id="df35f-109">Prerequisites</span></span> ##

<span data-ttu-id="df35f-110">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="df35f-110">To complete this tutorial:</span></span> 

* <span data-ttu-id="df35f-111">Installera den [.NET Core SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="df35f-111">Install the [.NET Core SDK](https://www.microsoft.com/net/download/core).</span></span>
* <span data-ttu-id="df35f-112">Installera [Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="df35f-112">Install [Git](https://git-scm.com/downloads).</span></span>

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-local-net-core-application"></a><span data-ttu-id="df35f-113">Skapa ett lokalt program i .NET Core</span><span class="sxs-lookup"><span data-stu-id="df35f-113">Create a local .NET Core application</span></span> ##

<span data-ttu-id="df35f-114">Starta en ny terminalsession.</span><span class="sxs-lookup"><span data-stu-id="df35f-114">Start a new terminal session.</span></span> <span data-ttu-id="df35f-115">Skapa en katalog med namnet `hellodotnetcore`, och ändra katalogen till den.</span><span class="sxs-lookup"><span data-stu-id="df35f-115">Create a directory named `hellodotnetcore`, and change the current directory to it.</span></span> <span data-ttu-id="df35f-116">Skriv följande:</span><span class="sxs-lookup"><span data-stu-id="df35f-116">Then type the following:</span></span> 

```
dotnet new web
``` 

  <span data-ttu-id="df35f-117">Det här kommandot skapar tre filer (*hellodotnetcore.csproj*, *Program.cs*, och *Startup.cs*) och en tom mapp (*wwwroot /*) under den aktuella katalogen.</span><span class="sxs-lookup"><span data-stu-id="df35f-117">This command creates three files (*hellodotnetcore.csproj*, *Program.cs*, and *Startup.cs*) and one empty folder (*wwwroot/*) under the current directory.</span></span> <span data-ttu-id="df35f-118">Innehållet i `.csproj` filen bör se ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="df35f-118">The content of `.csproj` file should look like the following:</span></span> 

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

<span data-ttu-id="df35f-119">Eftersom den här appen är ett webbprogram, en referens till ett ASP.NET Core-paket har lagts till automatiskt i *hellodotnetcore.csproj* fil.</span><span class="sxs-lookup"><span data-stu-id="df35f-119">Since this app is a web application, a reference to an ASP.NET Core package was automatically added to the *hellodotnetcore.csproj* file.</span></span> <span data-ttu-id="df35f-120">Versionsnummer för paketet anges enligt den valda framework.</span><span class="sxs-lookup"><span data-stu-id="df35f-120">The version number of the package is set according to the chosen framework.</span></span> <span data-ttu-id="df35f-121">Det här exemplet refererar till ASP.NET Core version 1.1.2 eftersom .NET Core 1.1 används.</span><span class="sxs-lookup"><span data-stu-id="df35f-121">This example is referencing ASP.NET Core version 1.1.2 because .NET Core 1.1 is used.</span></span>

## <a name="build-and-test-the-application-locally"></a><span data-ttu-id="df35f-122">Skapa och testa programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="df35f-122">Build and test the application locally</span></span> ##

<span data-ttu-id="df35f-123">Du kan skapa och köra appen med .NET Core den `dotnet restore` kommandot följt av den `dotnet run` kommandot som visas här:</span><span class="sxs-lookup"><span data-stu-id="df35f-123">You can build and run your .NET Core app with the `dotnet restore` command followed by the `dotnet run` command, as shown here:</span></span>

```
dotnet restore
dotnet run
```


<span data-ttu-id="df35f-124">När programmet startas, visar ett meddelande om appen lyssnar på inkommande begäranden på en port.</span><span class="sxs-lookup"><span data-stu-id="df35f-124">When the application starts, it displays a message indicating the app is listening to incoming requests at a port.</span></span> 

```bash
Hosting environment: Production
Content root path: C:\hellodotnetcore
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="df35f-125">Testa den genom att bläddra till `http://localhost:5000/` med din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="df35f-125">Test it by browsing to `http://localhost:5000/` with your browser.</span></span> <span data-ttu-id="df35f-126">Om allt fungerar bra visas ”Hello World”!</span><span class="sxs-lookup"><span data-stu-id="df35f-126">If everything works fine, you see "Hello World!"</span></span> <span data-ttu-id="df35f-127">som resultattext.</span><span class="sxs-lookup"><span data-stu-id="df35f-127">as the result text.</span></span>

![Testa med webbläsare][7]

## <a name="create-a-net-core-app-in-the-azure-portal"></a><span data-ttu-id="df35f-129">Skapa ett .NET Core app i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="df35f-129">Create a .NET Core app in the Azure Portal</span></span> ##

<span data-ttu-id="df35f-130">Du måste först skapa ett tomt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="df35f-130">First you need to create an empty web app.</span></span> <span data-ttu-id="df35f-131">Logga in på den [Azure-portalen](https://portal.azure.com/) och skapa en ny [webbprogrammet på Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span><span class="sxs-lookup"><span data-stu-id="df35f-131">Log in to the [Azure portal](https://portal.azure.com/) and create a new [Web App on Linux](https://portal.azure.com/#create/Microsoft.AppSvcLinux).</span></span>

![Skapa en webbapp][1]

<span data-ttu-id="df35f-133">När den **skapa** öppnas, innehåller information om ditt webbprogram:</span><span class="sxs-lookup"><span data-stu-id="df35f-133">When the **Create** page opens, provide details about your web app:</span></span>

![Om du väljer en .NET Core runtime stack][2]

<span data-ttu-id="df35f-135">Använd följande tabell som en vägledning för att fylla i den **skapa** sidan och välj sedan **OK** och **skapa** att skapa appen.</span><span class="sxs-lookup"><span data-stu-id="df35f-135">Use the following table as a guide to fill out the **Create** page, then select **OK** and **Create** to create the app.</span></span>

| <span data-ttu-id="df35f-136">Inställning</span><span class="sxs-lookup"><span data-stu-id="df35f-136">Setting</span></span>      | <span data-ttu-id="df35f-137">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="df35f-137">Suggested value</span></span>  | <span data-ttu-id="df35f-138">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="df35f-138">Description</span></span>                                        |
| ------------ | ---------------- | -------------------------------------------------- |
| <span data-ttu-id="df35f-139">Appnamn</span><span class="sxs-lookup"><span data-stu-id="df35f-139">App name</span></span> | <span data-ttu-id="df35f-140">hellodotnetcore</span><span class="sxs-lookup"><span data-stu-id="df35f-140">hellodotnetcore</span></span>  | <span data-ttu-id="df35f-141">Namnet på din app.</span><span class="sxs-lookup"><span data-stu-id="df35f-141">The name of your app.</span></span> <span data-ttu-id="df35f-142">Det här namnet måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="df35f-142">This name must be unique.</span></span> |
| <span data-ttu-id="df35f-143">Prenumeration</span><span class="sxs-lookup"><span data-stu-id="df35f-143">Subscription</span></span> | <span data-ttu-id="df35f-144">Välj en befintlig prenumeration</span><span class="sxs-lookup"><span data-stu-id="df35f-144">Choose an existing subscription</span></span> | <span data-ttu-id="df35f-145">Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="df35f-145">The Azure subscription.</span></span> |
| <span data-ttu-id="df35f-146">Resursgrupp</span><span class="sxs-lookup"><span data-stu-id="df35f-146">Resource Group</span></span> | <span data-ttu-id="df35f-147">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="df35f-147">myResourceGroup</span></span> |  <span data-ttu-id="df35f-148">Azure resursgruppen som innehåller webbappen.</span><span class="sxs-lookup"><span data-stu-id="df35f-148">The Azure resource group to contain the web app.</span></span> |
| <span data-ttu-id="df35f-149">App Service-plan</span><span class="sxs-lookup"><span data-stu-id="df35f-149">App Service Plan</span></span> | <span data-ttu-id="df35f-150">Namn på befintliga App Service-Plan</span><span class="sxs-lookup"><span data-stu-id="df35f-150">Existing App Service Plan name</span></span> |  <span data-ttu-id="df35f-151">App Service-plan.</span><span class="sxs-lookup"><span data-stu-id="df35f-151">The App Service plan.</span></span>  |
| <span data-ttu-id="df35f-152">Konfigurera behållare</span><span class="sxs-lookup"><span data-stu-id="df35f-152">Configure Container</span></span> | <span data-ttu-id="df35f-153">.NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="df35f-153">.NET Core 1.1</span></span> | <span data-ttu-id="df35f-154">Typ av behållare för det här webbprogrammet: inbyggda, Docker eller privata registret.</span><span class="sxs-lookup"><span data-stu-id="df35f-154">The type of container for this web app: Built-in, Docker, or Private registry.</span></span> |
| <span data-ttu-id="df35f-155">Bildens källa</span><span class="sxs-lookup"><span data-stu-id="df35f-155">Image source</span></span>  | <span data-ttu-id="df35f-156">Inbyggd</span><span class="sxs-lookup"><span data-stu-id="df35f-156">Built-in</span></span>  |  <span data-ttu-id="df35f-157">Bildens källa.</span><span class="sxs-lookup"><span data-stu-id="df35f-157">The source of the image.</span></span> |
| <span data-ttu-id="df35f-158">Runtime-stacken</span><span class="sxs-lookup"><span data-stu-id="df35f-158">Runtime Stack</span></span>  | <span data-ttu-id="df35f-159">.NET core 1.1</span><span class="sxs-lookup"><span data-stu-id="df35f-159">.NET Core 1.1</span></span>  | <span data-ttu-id="df35f-160">Runtime-stacken och version.</span><span class="sxs-lookup"><span data-stu-id="df35f-160">The runtime stack and version.</span></span>  |

## <a name="deploy-your-application-via-git"></a><span data-ttu-id="df35f-161">Distribuera programmet via Git</span><span class="sxs-lookup"><span data-stu-id="df35f-161">Deploy your application via Git</span></span> ##

<span data-ttu-id="df35f-162">Använda Git för att distribuera programmet till Azure App Service Web App på Linux .NET Core.</span><span class="sxs-lookup"><span data-stu-id="df35f-162">Use Git to deploy the .NET Core application to Azure App Service Web App on Linux.</span></span>

<span data-ttu-id="df35f-163">Nya Azure-webbappen har redan konfigurerats för Git-distribution.</span><span class="sxs-lookup"><span data-stu-id="df35f-163">The new Azure web app already has Git deployment configured.</span></span> <span data-ttu-id="df35f-164">Du hittar URL för Git-distribution genom att navigera till följande URL när du har infogat din webbprogrammets namn:</span><span class="sxs-lookup"><span data-stu-id="df35f-164">You will find the Git deployment URL by navigating to the following URL after inserting your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/api/scm/info```

<span data-ttu-id="df35f-165">Git-URL har följande format baserat på din webbprogrammets namn:</span><span class="sxs-lookup"><span data-stu-id="df35f-165">The Git URL has the following form based on your web app name:</span></span>

```https://{your web app name}.scm.azurewebsites.net/{your web app name}.git```

<span data-ttu-id="df35f-166">Kör följande kommandon för att distribuera lokala programmet till Azure webbapp:</span><span class="sxs-lookup"><span data-stu-id="df35f-166">Run the following commands to deploy the local application to your Azure web app:</span></span> 
 
```bash
git init
git remote add azure <Git deployment URL from above>
git add *.csproj *.cs
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="df35f-167">Du behöver inte push alla filer under *bin /* eller *obj /* kataloger eftersom programmet är inbyggd i molnet när programmets källfiler pushas till Azure.</span><span class="sxs-lookup"><span data-stu-id="df35f-167">You don't need to push any files under *bin/* or *obj/* directories because your application is built in the cloud when the application's source files are pushed to Azure.</span></span> <span data-ttu-id="df35f-168">När build-processen är klar, binära filer kopieras till programmets katalog vid */home/plats/wwwroot/*.</span><span class="sxs-lookup"><span data-stu-id="df35f-168">After the build process is complete, binary files are copied into the application's directory at */home/site/wwwroot/*.</span></span>

<span data-ttu-id="df35f-169">Bekräfta att fjärråtkomst distributionsåtgärder rapporterar lyckades.</span><span class="sxs-lookup"><span data-stu-id="df35f-169">Confirm that the remote deployment operations report success.</span></span> <span data-ttu-id="df35f-170">Push-åtgärder kan ta ett tag sedan paketet upplösning och skapa process som körs i molnet.</span><span class="sxs-lookup"><span data-stu-id="df35f-170">Push operations may take a while since package resolution and build process run in the cloud.</span></span> <span data-ttu-id="df35f-171">Du ser flera statusmeddelanden, inklusive de som anger att filerna har kopierats.</span><span class="sxs-lookup"><span data-stu-id="df35f-171">You will see several status messages, including ones stating that files have been copied.</span></span> <span data-ttu-id="df35f-172">Resultatet bör likna följande:</span><span class="sxs-lookup"><span data-stu-id="df35f-172">The output should look similar to the following:</span></span>

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
To https://hellodotnetcore.scm.azurewebsites.net/
 * [new branch]           master -> master

```

<span data-ttu-id="df35f-173">När distributionen är klar startar du om ditt webbprogram för att distributionen ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="df35f-173">Once the deployment has completed, restart your web app for the deployment to take effect.</span></span> <span data-ttu-id="df35f-174">Gör du genom att gå till Azure-portalen och gå till den **översikt** sidan av ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="df35f-174">To do this, go to the Azure portal and navigate to the **Overview** page of your web app.</span></span> <span data-ttu-id="df35f-175">Välj den **starta om** knappen på sidan.</span><span class="sxs-lookup"><span data-stu-id="df35f-175">Select the **Restart** button in the page.</span></span> <span data-ttu-id="df35f-176">När ett popup-fönster visas, väljer du **Ja** att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="df35f-176">When a popup window shows up, select **Yes** to confirm.</span></span> <span data-ttu-id="df35f-177">Du kan sedan bläddra ditt webbprogram som visas här:</span><span class="sxs-lookup"><span data-stu-id="df35f-177">You can then browse your web app, as shown here:</span></span>

![.NET Core app distribueras till Azure App Service på Linux-surfning][10]

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="df35f-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="df35f-179">Next steps</span></span>
* [<span data-ttu-id="df35f-180">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="df35f-180">Azure App Service Web App on Linux FAQ</span></span>](./app-service-linux-faq.md)

[1]: ./media/app-service-linux-using-dotnetcore/top-level-create.png
[2]: ./media/app-service-linux-using-dotnetcore/dotnet-new-webapp.png
[7]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-local.png
[10]: ./media/app-service-linux-using-dotnetcore/dotnet-browse-azure.png
