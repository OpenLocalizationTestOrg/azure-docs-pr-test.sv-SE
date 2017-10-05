---
title: Skapa en Node.js-webbapp i Azure | Microsoft Docs
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
ms.openlocfilehash: ce845da09a7c088b8a2ba29b818a46a3b41aa4e7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-nodejs-web-app-in-azure"></a><span data-ttu-id="79284-103">Skapa en Node.js-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="79284-103">Create a Node.js web app in Azure</span></span>

<span data-ttu-id="79284-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="79284-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="79284-105">Den här snabbstarten visar hur du distribuerar en Node.js-app till Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="79284-105">This quickstart shows how to deploy a Node.js app to Azure Web Apps.</span></span> <span data-ttu-id="79284-106">Du skapar webbappen med [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) och använder Git för att distribuera Node.js-exempelkoden till webbappen.</span><span class="sxs-lookup"><span data-stu-id="79284-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git to deploy sample Node.js code to the web app.</span></span>

![Exempelapp som körs i Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="79284-108">Du kan följa stegen nedan på en Mac-, Windows- eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="79284-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="79284-109">Det tar cirka fem minuter att slutföra självstudiekursen när de nödvändiga komponenterna har installerats.</span><span class="sxs-lookup"><span data-stu-id="79284-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   


## <a name="prerequisites"></a><span data-ttu-id="79284-110">Krav</span><span class="sxs-lookup"><span data-stu-id="79284-110">Prerequisites</span></span>

<span data-ttu-id="79284-111">För att slutföra den här snabbstarten behöver du:</span><span class="sxs-lookup"><span data-stu-id="79284-111">To complete this quickstart:</span></span>

* [<span data-ttu-id="79284-112">Installera Git</span><span class="sxs-lookup"><span data-stu-id="79284-112">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="79284-113">Installera Node.js och NPM</span><span class="sxs-lookup"><span data-stu-id="79284-113">Install Node.js and NPM</span></span>](https://nodejs.org/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="79284-114">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="79284-114">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="79284-115">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="79284-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="79284-116">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="79284-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-the-sample"></a><span data-ttu-id="79284-117">Hämta exemplet</span><span class="sxs-lookup"><span data-stu-id="79284-117">Download the sample</span></span>

<span data-ttu-id="79284-118">Kör följande kommando i ett terminalfönster för att klona databasen för exempelappen till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="79284-118">In a terminal window, run the following command to clone the sample app repository to your local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world
```

<span data-ttu-id="79284-119">Du använder det här terminalfönstret för att köra alla kommandon i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="79284-119">You use this terminal window to run all the commands in this quickstart.</span></span>

<span data-ttu-id="79284-120">Ändra till den katalog som innehåller exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="79284-120">Change to the directory that contains the sample code.</span></span>

```bash
cd nodejs-docs-hello-world
```

## <a name="run-the-app-locally"></a><span data-ttu-id="79284-121">Köra appen lokalt</span><span class="sxs-lookup"><span data-stu-id="79284-121">Run the app locally</span></span>

<span data-ttu-id="79284-122">Kör programmet lokalt genom att öppna ett terminalfönster och använda `npm start`-skriptet för att starta den inbyggda Node.js HTTP-servern.</span><span class="sxs-lookup"><span data-stu-id="79284-122">Run the application locally by opening a terminal window and using the `npm start` script to launch the built in Node.js HTTP server.</span></span>

```bash
npm start
```

<span data-ttu-id="79284-123">Öppna en webbläsare och navigera till exempelappen på http://localhost:1337.</span><span class="sxs-lookup"><span data-stu-id="79284-123">Open a web browser, and navigate to the sample app at http://localhost:1337.</span></span>

<span data-ttu-id="79284-124">Nu kan du se **Hello World**-meddelandet från exempelappen på sidan.</span><span class="sxs-lookup"><span data-stu-id="79284-124">You see the **Hello World** message from the sample app displayed in the page.</span></span>

![Exempelapp som körs lokalt](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

<span data-ttu-id="79284-126">Tryck på **Ctrl+C** i terminalfönstret för att avsluta webbservern.</span><span class="sxs-lookup"><span data-stu-id="79284-126">In your terminal window, press **Ctrl+C** to exit the web server.</span></span>

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Sida för tom webbapp](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="79284-128">Nu har du skapat en ny tom webbapp på Azure.</span><span class="sxs-lookup"><span data-stu-id="79284-128">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 23, done.
Delta compression using up to 4 threads.
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
remote: Node.js versions available on the platform are: 4.4.7, 4.5.0, 6.2.2, 6.6.0, 6.9.1.
remote: Selected node.js version 6.9.1. Use package.json file to choose a different version.
remote: Selected npm version 3.10.8
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net:443/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a><span data-ttu-id="79284-129">Bläddra till appen</span><span class="sxs-lookup"><span data-stu-id="79284-129">Browse to the app</span></span>

<span data-ttu-id="79284-130">Bläddra till den distribuerade appen via webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="79284-130">Browse to the deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="79284-131">Node.js-exempelkoden körs i en Azure App Service-webbapp.</span><span class="sxs-lookup"><span data-stu-id="79284-131">The Node.js sample code is running in an Azure App Service web app.</span></span>

![Exempelapp som körs i Azure](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

<span data-ttu-id="79284-133">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="79284-133">**Congratulations!**</span></span> <span data-ttu-id="79284-134">Du har distribuerat din första Node.js-app till App Service.</span><span class="sxs-lookup"><span data-stu-id="79284-134">You've deployed your first Node.js app to App Service.</span></span>

## <a name="update-and-redeploy-the-code"></a><span data-ttu-id="79284-135">Uppdatera och distribuera om koden</span><span class="sxs-lookup"><span data-stu-id="79284-135">Update and redeploy the code</span></span>

<span data-ttu-id="79284-136">Öppna filen `index.js` i Node.js-appen med ett textredigeringsprogram och gör små ändringar i texten i anropet till `response.end`:</span><span class="sxs-lookup"><span data-stu-id="79284-136">Using a text editor, open the `index.js` file in the Node.js app, and make a small change to the text in the call to `response.end`:</span></span>

```nodejs
response.end("Hello Azure!");
```

<span data-ttu-id="79284-137">Spara ändringarna på Git och skicka sedan kodändringarna till Azure.</span><span class="sxs-lookup"><span data-stu-id="79284-137">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="79284-138">När distributionen är klar går du tillbaka till webbläsarfönstret som öppnades när du skulle **söka efter appen** och klickar på knappen för att uppdatera.</span><span class="sxs-lookup"><span data-stu-id="79284-138">Once deployment has completed, switch back to the browser window that opened in the **Browse to the app** step, and hit refresh.</span></span>

![Uppdaterad exempelapp som körs i Azure](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="79284-140">Hantera din nya Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="79284-140">Manage your new Azure web app</span></span>

<span data-ttu-id="79284-141">Gå till <a href="https://portal.azure.com" target="_blank">Azure Portal</a> för att hantera den webbapp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="79284-141">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="79284-142">Klicka på **Apptjänster** i menyn till vänster och sedan på namnet på din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="79284-142">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigera till webbappen på Azure Portal](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="79284-144">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="79284-144">You see your web app's Overview page.</span></span> <span data-ttu-id="79284-145">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="79284-145">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![App Service-blad på Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="79284-147">Menyn till vänster innehåller olika sidor för att konfigurera appen.</span><span class="sxs-lookup"><span data-stu-id="79284-147">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="79284-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79284-148">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="79284-149">Node.js med MongoDB</span><span class="sxs-lookup"><span data-stu-id="79284-149">Node.js with MongoDB</span></span>](app-service-web-tutorial-nodejs-mongodb-app.md)
