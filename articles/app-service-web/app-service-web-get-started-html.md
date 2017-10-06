---
title: aaaCreate statiska HTML-webbapp i Azure | Microsoft Docs
description: "Lär dig hur toorun web apps i Azure App Service genom att distribuera en statisk HTML sample-appen."
services: app-service\web
documentationcenter: 
author: rick-anderson
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 05/26/2017
ms.author: riande
ms.custom: mvc
ms.openlocfilehash: efd8c8189a3aa1ac35602b688eeb31bff6f5a373
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-static-html-web-app-in-azure"></a><span data-ttu-id="08ec7-103">Skapa en statisk HTML-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="08ec7-103">Create a static HTML web app in Azure</span></span>

<span data-ttu-id="08ec7-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="08ec7-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="08ec7-105">Den här snabbstarten visar hur toodeploy grundläggande HTML + CSS plats tooAzure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="08ec7-105">This quickstart shows how toodeploy a basic HTML+CSS site tooAzure Web Apps.</span></span> <span data-ttu-id="08ec7-106">Du skapar hello webbapp med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), och du kan använda Git toodeploy HTML-innehåll toohello exempelwebbapp.</span><span class="sxs-lookup"><span data-stu-id="08ec7-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git toodeploy sample HTML content toohello web app.</span></span>

![Startsida för exempelapp](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="08ec7-108">Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="08ec7-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="08ec7-109">När hello nödvändiga komponenter har installerats, tar cirka fem minuter toocomplete hello steg.</span><span class="sxs-lookup"><span data-stu-id="08ec7-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08ec7-110">Krav</span><span class="sxs-lookup"><span data-stu-id="08ec7-110">Prerequisites</span></span>

<span data-ttu-id="08ec7-111">toocomplete denna Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="08ec7-111">toocomplete this quickstart:</span></span>

- <span data-ttu-id="08ec7-112">[Installera Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]</span><span class="sxs-lookup"><span data-stu-id="08ec7-112">[Install Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="08ec7-113">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="08ec7-113">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="08ec7-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="08ec7-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="08ec7-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="08ec7-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-hello-sample"></a><span data-ttu-id="08ec7-116">Hämta hello-exempel</span><span class="sxs-lookup"><span data-stu-id="08ec7-116">Download hello sample</span></span>

<span data-ttu-id="08ec7-117">Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="08ec7-117">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

<span data-ttu-id="08ec7-118">Du använder den här terminalfönster toorun alla hello-kommandon i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="08ec7-118">You use this terminal window toorun all hello commands in this quickstart.</span></span>

## <a name="view-hello-html"></a><span data-ttu-id="08ec7-119">Visa hello HTML</span><span class="sxs-lookup"><span data-stu-id="08ec7-119">View hello HTML</span></span>

<span data-ttu-id="08ec7-120">Navigera toohello katalog som innehåller exempel på hello HTML.</span><span class="sxs-lookup"><span data-stu-id="08ec7-120">Navigate toohello directory that contains hello sample HTML.</span></span> <span data-ttu-id="08ec7-121">Öppna hello *index.html* filen i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="08ec7-121">Open hello *index.html* file in your browser.</span></span>

![Exempelstartsida för app](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Sida för tom webbapp](media/app-service-web-get-started-html/app-service-web-service-created.png)

<span data-ttu-id="08ec7-124">Nu har du skapat en ny tom webbapp på Azure.</span><span class="sxs-lookup"><span data-stu-id="08ec7-124">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 13, done.
Delta compression using up too4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a><span data-ttu-id="08ec7-125">Bläddra toohello app</span><span class="sxs-lookup"><span data-stu-id="08ec7-125">Browse toohello app</span></span>

<span data-ttu-id="08ec7-126">Gå toohello Azure web app-URL i en webbläsare:</span><span class="sxs-lookup"><span data-stu-id="08ec7-126">In a browser, go toohello Azure web app URL:</span></span>

```
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="08ec7-127">hello sidan körs som en webbapp i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="08ec7-127">hello page is running as an Azure App Service web app.</span></span>

![Exempelstartsida för app](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="08ec7-129">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="08ec7-129">**Congratulations!**</span></span> <span data-ttu-id="08ec7-130">Du har distribuerat din första HTML-app tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="08ec7-130">You've deployed your first HTML app tooApp Service.</span></span>

## <a name="update-and-redeploy-hello-app"></a><span data-ttu-id="08ec7-131">Uppdatera och distribuera hello app</span><span class="sxs-lookup"><span data-stu-id="08ec7-131">Update and redeploy hello app</span></span>

<span data-ttu-id="08ec7-132">Öppna hello *index.html* filen i en textredigerare och gör en ändring toohello markeringar.</span><span class="sxs-lookup"><span data-stu-id="08ec7-132">Open hello *index.html* file in a text editor, and make a change toohello markup.</span></span> <span data-ttu-id="08ec7-133">Ändra exempelvis hello H1 rubriken ”Azure App Service – statiska HTML-Exempelplats” toojust ”Azure App Service”.</span><span class="sxs-lookup"><span data-stu-id="08ec7-133">For example, change hello H1 heading from "Azure App Service - Sample Static HTML Site" toojust "Azure App Service\`.</span></span>

<span data-ttu-id="08ec7-134">Genomför ändringarna i Git och skicka sedan hello kod ändringar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="08ec7-134">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated HTML"
git push azure master
```

<span data-ttu-id="08ec7-135">Uppdatera din webbläsare toosee hello ändringar när distributionen är klar.</span><span class="sxs-lookup"><span data-stu-id="08ec7-135">Once deployment has completed, refresh your browser toosee hello changes.</span></span>

![Uppdaterad exempelstartsida för app](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="08ec7-137">Hantera din nya Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="08ec7-137">Manage your new Azure web app</span></span>

<span data-ttu-id="08ec7-138">Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toomanage hello webbprogram som du skapade.</span><span class="sxs-lookup"><span data-stu-id="08ec7-138">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="08ec7-139">Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="08ec7-139">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-get-started-html/portal1.png)

<span data-ttu-id="08ec7-141">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="08ec7-141">You see your web app's Overview page.</span></span> <span data-ttu-id="08ec7-142">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="08ec7-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![App Service-blad på Azure Portal](./media/app-service-web-get-started-html/portal2.png)

<span data-ttu-id="08ec7-144">hello vänstra menyn innehåller olika sidor för att konfigurera din app.</span><span class="sxs-lookup"><span data-stu-id="08ec7-144">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="08ec7-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="08ec7-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="08ec7-146">Mappa anpassad domän</span><span class="sxs-lookup"><span data-stu-id="08ec7-146">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
