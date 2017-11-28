---
title: aaaCreate en Node.js-webbapp i Azure | Microsoft Docs
description: "Distribuera din första Node.js-Hello World-app i Azure App Service Web Apps på bara några minuter."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 05/05/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 163edf83b2353755fc9fa2d75aed489038cf7c81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-nodejs-web-app-in-azure"></a><span data-ttu-id="376b9-103">Skapa en Node.js-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="376b9-103">Create a Node.js web app in Azure</span></span>

<span data-ttu-id="376b9-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="376b9-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="376b9-105">Den här snabbstarten visar hur toodeploy en Node.js-app tooAzure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="376b9-105">This quickstart shows how toodeploy a Node.js app tooAzure Web Apps.</span></span> <span data-ttu-id="376b9-106">Du skapar hello webbapp med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), och du kan använda Git toodeploy Node.js-kod toohello exempelwebbapp.</span><span class="sxs-lookup"><span data-stu-id="376b9-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git toodeploy sample Node.js code toohello web app.</span></span>

![Exempelapp som körs i Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="376b9-108">Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="376b9-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="376b9-109">När hello nödvändiga komponenter har installerats, tar cirka fem minuter toocomplete hello steg.</span><span class="sxs-lookup"><span data-stu-id="376b9-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   


## <a name="prerequisites"></a><span data-ttu-id="376b9-110">Krav</span><span class="sxs-lookup"><span data-stu-id="376b9-110">Prerequisites</span></span>

<span data-ttu-id="376b9-111">toocomplete denna Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="376b9-111">toocomplete this quickstart:</span></span>

* [<span data-ttu-id="376b9-112">Installera Git</span><span class="sxs-lookup"><span data-stu-id="376b9-112">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="376b9-113">Installera Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="376b9-113">Install Node.js and NPM</span></span>](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="376b9-114">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="376b9-114">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="376b9-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="376b9-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="376b9-116">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="376b9-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-hello-sample"></a><span data-ttu-id="376b9-117">Hämta hello-exempel</span><span class="sxs-lookup"><span data-stu-id="376b9-117">Download hello sample</span></span>

<span data-ttu-id="376b9-118">Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="376b9-118">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

<span data-ttu-id="376b9-119">Du använder den här terminalfönster toorun alla hello-kommandon i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="376b9-119">You use this terminal window toorun all hello commands in this quickstart.</span></span>

<span data-ttu-id="376b9-120">Ändra toohello katalog som innehåller hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="376b9-120">Change toohello directory that contains hello sample code.</span></span>

```bash
cd nodejs-docs-hello-world
```

## <a name="run-hello-app-locally"></a><span data-ttu-id="376b9-121">Kör hello appen lokalt</span><span class="sxs-lookup"><span data-stu-id="376b9-121">Run hello app locally</span></span>

<span data-ttu-id="376b9-122">Kör programmet hello lokalt genom att öppna ett terminalfönster och hello `npm start` skriptet toolaunch hello inbyggda Node.js HTTP-servern.</span><span class="sxs-lookup"><span data-stu-id="376b9-122">Run hello application locally by opening a terminal window and using hello `npm start` script toolaunch hello built in Node.js HTTP server.</span></span>

```bash
npm start
```

<span data-ttu-id="376b9-123">Öppna en webbläsare och gå toohello sample-appen på http://localhost: 1337.</span><span class="sxs-lookup"><span data-stu-id="376b9-123">Open a web browser, and navigate toohello sample app at http://localhost:1337.</span></span>

<span data-ttu-id="376b9-124">Du ser hello **Hello World** meddelande från hello sample-appen visas i hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="376b9-124">You see hello **Hello World** message from hello sample app displayed in hello page.</span></span>

![Exempelapp som körs lokalt](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

<span data-ttu-id="376b9-126">Tryck på i ditt terminalfönster **Ctrl + C** tooexit hello-webbserver.</span><span class="sxs-lookup"><span data-stu-id="376b9-126">In your terminal window, press **Ctrl+C** tooexit hello web server.</span></span>

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Sida för tom webbapp](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="376b9-128">Nu har du skapat en ny tom webbapp på Azure.</span><span class="sxs-lookup"><span data-stu-id="376b9-128">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 23, done.
Delta compression using up too4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.71 KiB | 0 bytes/s, done.
Total 23 (delta 8), reused 7 (delta 1)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'bf114df591'.
remote: Generating deployment script.
remote: Generating deployment script for node.js Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling node.js deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.js'
remote: Copying file: 'package.json'
remote: Copying file: 'process.json'
remote: Deleting file: 'hostingstart.html'
remote: Ignoring: .git
remote: Using start-up script index.js from package.json.
remote: Node.js versions available on hello platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file toochoose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a><span data-ttu-id="376b9-129">Bläddra toohello app</span><span class="sxs-lookup"><span data-stu-id="376b9-129">Browse toohello app</span></span>

<span data-ttu-id="376b9-130">Bläddra toohello distribuerat program med hjälp av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="376b9-130">Browse toohello deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="376b9-131">Hej Node.js exempelkod körs i en webbapp i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="376b9-131">hello Node.js sample code is running in an Azure App Service web app.</span></span>

![Exempelapp som körs i Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="376b9-133">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="376b9-133">**Congratulations!**</span></span> <span data-ttu-id="376b9-134">Du har distribuerat din första Node.js app tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="376b9-134">You've deployed your first Node.js app tooApp Service.</span></span>

## <a name="update-and-redeploy-hello-code"></a><span data-ttu-id="376b9-135">Uppdatera och distribuera hello kod</span><span class="sxs-lookup"><span data-stu-id="376b9-135">Update and redeploy hello code</span></span>

<span data-ttu-id="376b9-136">Använd en textredigerare och öppna hello `index.js` filen i hello Node.js-app och göra en mindre ändring toohello text i hello anrop för`response.end`:</span><span class="sxs-lookup"><span data-stu-id="376b9-136">Using a text editor, open hello `index.js` file in hello Node.js app, and make a small change toohello text in hello call too`response.end`:</span></span>

```nodejs
response.end("Hello Azure!");
```

<span data-ttu-id="376b9-137">Genomför ändringarna i Git och skicka sedan hello kod ändringar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="376b9-137">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="376b9-138">När distributionen är klar, kan du växla tillbaka toohello webbläsarfönster som öppnas i hello **Bläddra toohello app** steg och klicka på Uppdatera.</span><span class="sxs-lookup"><span data-stu-id="376b9-138">Once deployment has completed, switch back toohello browser window that opened in hello **Browse toohello app** step, and hit refresh.</span></span>

![Uppdaterad exempelapp som körs i Azure](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="376b9-140">Hantera din nya Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="376b9-140">Manage your new Azure web app</span></span>

<span data-ttu-id="376b9-141">Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toomanage hello webbprogram som du skapade.</span><span class="sxs-lookup"><span data-stu-id="376b9-141">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="376b9-142">Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="376b9-142">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="376b9-144">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="376b9-144">You see your web app's Overview page.</span></span> <span data-ttu-id="376b9-145">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="376b9-145">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![App Service-blad på Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="376b9-147">hello vänstra menyn innehåller olika sidor för att konfigurera din app.</span><span class="sxs-lookup"><span data-stu-id="376b9-147">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="376b9-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="376b9-148">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="376b9-149">Node.js med MongoDB</span><span class="sxs-lookup"><span data-stu-id="376b9-149">Node.js with MongoDB</span></span>](app-service-web-tutorial-nodejs-mongodb-app.md)
