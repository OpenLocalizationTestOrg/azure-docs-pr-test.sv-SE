---
title: Skapa en statisk HTML-webbapp i Azure | Microsoft Docs
description: "Distribuera en statisk HTML-exempelapp och lär dig att köra webbappar i Azure App Service."
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
ms.openlocfilehash: 42af5b08b8d2ff0c75fd73dcfa61c861647fd2c9
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-static-html-web-app-in-azure"></a><span data-ttu-id="db030-103">Skapa en statisk HTML-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="db030-103">Create a static HTML web app in Azure</span></span>

<span data-ttu-id="db030-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="db030-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="db030-105">Den här snabbstarten visar hur du distribuerar en enkel HTML+CSS-webbplats till Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="db030-105">This quickstart shows how to deploy a basic HTML+CSS site to Azure Web Apps.</span></span> <span data-ttu-id="db030-106">Du skapar webbappen med [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) och använder Git för att distribuera HTML-innehåll till webbappen.</span><span class="sxs-lookup"><span data-stu-id="db030-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git to deploy sample HTML content to the web app.</span></span>

![Startsida för exempelapp](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="db030-108">Du kan följa stegen nedan på en Mac-, Windows- eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="db030-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="db030-109">Det tar cirka fem minuter att slutföra självstudiekursen när de nödvändiga komponenterna har installerats.</span><span class="sxs-lookup"><span data-stu-id="db030-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="db030-110">Krav</span><span class="sxs-lookup"><span data-stu-id="db030-110">Prerequisites</span></span>

<span data-ttu-id="db030-111">För att slutföra den här snabbstarten behöver du:</span><span class="sxs-lookup"><span data-stu-id="db030-111">To complete this quickstart:</span></span>

- <span data-ttu-id="db030-112">[Installera Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]</span><span class="sxs-lookup"><span data-stu-id="db030-112">[Install Git](https://git-scm.com/)
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="db030-113">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="db030-113">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="db030-114">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="db030-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="db030-115">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="db030-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-the-sample"></a><span data-ttu-id="db030-116">Hämta exemplet</span><span class="sxs-lookup"><span data-stu-id="db030-116">Download the sample</span></span>

<span data-ttu-id="db030-117">Kör följande kommando i ett terminalfönster för att klona databasen för exempelappen till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="db030-117">In a terminal window, run the following command to clone the sample app repository to your local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

<span data-ttu-id="db030-118">Du använder det här terminalfönstret för att köra alla kommandon i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="db030-118">You use this terminal window to run all the commands in this quickstart.</span></span>

## <a name="view-the-html"></a><span data-ttu-id="db030-119">Visa HTML</span><span class="sxs-lookup"><span data-stu-id="db030-119">View the HTML</span></span>

<span data-ttu-id="db030-120">Navigera till den katalog som innehåller HTML-exemplet.</span><span class="sxs-lookup"><span data-stu-id="db030-120">Navigate to the directory that contains the sample HTML.</span></span> <span data-ttu-id="db030-121">Öppna filen *index.html* i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="db030-121">Open the *index.html* file in your browser.</span></span>

![Exempelstartsida för app](media/app-service-web-get-started-html/hello-world-in-browser.png)

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Sida för tom webbapp](media/app-service-web-get-started-html/app-service-web-service-created.png)

<span data-ttu-id="db030-124">Nu har du skapat en ny tom webbapp på Azure.</span><span class="sxs-lookup"><span data-stu-id="db030-124">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 13, done.
Delta compression using up to 4 threads.
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
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a><span data-ttu-id="db030-125">Bläddra till appen</span><span class="sxs-lookup"><span data-stu-id="db030-125">Browse to the app</span></span>

<span data-ttu-id="db030-126">Gå till Azure-webbappens URL i en webbläsare:</span><span class="sxs-lookup"><span data-stu-id="db030-126">In a browser, go to the Azure web app URL:</span></span>

```
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="db030-127">Sidan körs som en Azure App Service-webbapp.</span><span class="sxs-lookup"><span data-stu-id="db030-127">The page is running as an Azure App Service web app.</span></span>

![Exempelstartsida för app](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

<span data-ttu-id="db030-129">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="db030-129">**Congratulations!**</span></span> <span data-ttu-id="db030-130">Du har distribuerat din första HTML-app till App Service.</span><span class="sxs-lookup"><span data-stu-id="db030-130">You've deployed your first HTML app to App Service.</span></span>

## <a name="update-and-redeploy-the-app"></a><span data-ttu-id="db030-131">Uppdatera och distribuera om appen</span><span class="sxs-lookup"><span data-stu-id="db030-131">Update and redeploy the app</span></span>

<span data-ttu-id="db030-132">Öppna filen *index.html* i en textredigerare och gör en ändring i koden.</span><span class="sxs-lookup"><span data-stu-id="db030-132">Open the *index.html* file in a text editor, and make a change to the markup.</span></span> <span data-ttu-id="db030-133">Ändra till exempel H1-rubriken från "Azure App Service - Sample Static HTML Site" till endast "Azure App Service".</span><span class="sxs-lookup"><span data-stu-id="db030-133">For example, change the H1 heading from "Azure App Service - Sample Static HTML Site" to just "Azure App Service\`.</span></span>

<span data-ttu-id="db030-134">Spara ändringarna på Git och skicka sedan kodändringarna till Azure.</span><span class="sxs-lookup"><span data-stu-id="db030-134">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated HTML"
git push azure master
```

<span data-ttu-id="db030-135">När distributionen är klar väljer du att uppdatera i webbläsaren så att ändringarna visas.</span><span class="sxs-lookup"><span data-stu-id="db030-135">Once deployment has completed, refresh your browser to see the changes.</span></span>

![Uppdaterad exempelstartsida för app](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="db030-137">Hantera din nya Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="db030-137">Manage your new Azure web app</span></span>

<span data-ttu-id="db030-138">Gå till <a href="https://portal.azure.com" target="_blank">Azure Portal</a> för att hantera den webbapp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="db030-138">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="db030-139">Klicka på **Apptjänster** i menyn till vänster och sedan på namnet på din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="db030-139">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigera till webbappen på Azure Portal](./media/app-service-web-get-started-html/portal1.png)

<span data-ttu-id="db030-141">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="db030-141">You see your web app's Overview page.</span></span> <span data-ttu-id="db030-142">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="db030-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![App Service-blad på Azure Portal](./media/app-service-web-get-started-html/portal2.png)

<span data-ttu-id="db030-144">Menyn till vänster innehåller olika sidor för att konfigurera appen.</span><span class="sxs-lookup"><span data-stu-id="db030-144">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="db030-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db030-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="db030-146">Mappa anpassad domän</span><span class="sxs-lookup"><span data-stu-id="db030-146">Map custom domain</span></span>](app-service-web-tutorial-custom-domain.md)
