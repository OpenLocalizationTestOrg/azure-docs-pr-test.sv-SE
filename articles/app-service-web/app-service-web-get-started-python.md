---
title: Skapa en Python-webbapp i Azure | Microsoft Docs
description: "Distribuera din första Hello World-app med Python i Azure App Service Web Apps på bara några minuter."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 928ee2e5-6143-4c0c-8546-366f5a3d80ce
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/17/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 119f9770097c010cc360e0e204d06a307a268814
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-python-web-app-in-azure"></a><span data-ttu-id="c3410-103">Skapa en Python-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="c3410-103">Create a Python web app in Azure</span></span>

<span data-ttu-id="c3410-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="c3410-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="c3410-105">Den här snabbstarten visar hur du utvecklar en Python-app och distribuerar den till Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="c3410-105">This quickstart walks through how to develop and deploy a Python app to Azure Web Apps.</span></span> <span data-ttu-id="c3410-106">Du skapar webbappen med [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) och använder Git för att distribuera Python-exempelkoden till webbappen.</span><span class="sxs-lookup"><span data-stu-id="c3410-106">You create the web app using the [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git to deploy sample Python code to the web app.</span></span>

![Exempelapp som körs i Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="c3410-108">Du kan följa stegen nedan på en Mac-, Windows- eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="c3410-108">You can follow the steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="c3410-109">Det tar cirka fem minuter att slutföra självstudiekursen när de nödvändiga komponenterna har installerats.</span><span class="sxs-lookup"><span data-stu-id="c3410-109">Once the prerequisites are installed, it takes about five minutes to complete the steps.</span></span>
## <a name="prerequisites"></a><span data-ttu-id="c3410-110">Krav</span><span class="sxs-lookup"><span data-stu-id="c3410-110">Prerequisites</span></span>

<span data-ttu-id="c3410-111">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="c3410-111">To complete this tutorial:</span></span>

1. [<span data-ttu-id="c3410-112">Installera Git</span><span class="sxs-lookup"><span data-stu-id="c3410-112">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="c3410-113">Installera Python</span><span class="sxs-lookup"><span data-stu-id="c3410-113">Install Python</span></span>](https://www.python.org/downloads/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c3410-114">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c3410-114">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="c3410-115">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="c3410-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="c3410-116">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c3410-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-the-sample"></a><span data-ttu-id="c3410-117">Hämta exemplet</span><span class="sxs-lookup"><span data-stu-id="c3410-117">Download the sample</span></span>

<span data-ttu-id="c3410-118">Kör följande kommando i ett terminalfönster för att klona databasen för exempelappen till den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="c3410-118">In a terminal window, run the following command to clone the sample app repository to your local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

<span data-ttu-id="c3410-119">Du använder det här terminalfönstret för att köra alla kommandon i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="c3410-119">You use this terminal window to run all the commands in this quickstart.</span></span>

<span data-ttu-id="c3410-120">Ändra till den katalog som innehåller exempelkoden.</span><span class="sxs-lookup"><span data-stu-id="c3410-120">Change to the directory that contains the sample code.</span></span>

```bash
cd Python-docs-hello-world
```

## <a name="run-the-app-locally"></a><span data-ttu-id="c3410-121">Köra appen lokalt</span><span class="sxs-lookup"><span data-stu-id="c3410-121">Run the app locally</span></span>

<span data-ttu-id="c3410-122">Installera de paket som behövs med hjälp av `pip`.</span><span class="sxs-lookup"><span data-stu-id="c3410-122">Install the required packages using `pip`.</span></span>

```bash
pip install -r requirements.txt
```

<span data-ttu-id="c3410-123">Kör programmet lokalt genom att öppna ett terminalfönster och använda kommandot `Python` för att starta den inbyggda Python-webbservern.</span><span class="sxs-lookup"><span data-stu-id="c3410-123">Run the application locally by opening a terminal window and using the `Python` command to launch the built-in Python web server.</span></span>

```bash
python main.py
```

<span data-ttu-id="c3410-124">Öppna en webbläsare och navigera till exempelappen på http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="c3410-124">Open a web browser, and navigate to the sample app at http://localhost:5000.</span></span>

<span data-ttu-id="c3410-125">Nu kan du se **Hello World**-meddelandet från exempelappen på sidan.</span><span class="sxs-lookup"><span data-stu-id="c3410-125">You can see the **Hello World** message from the sample app displayed in the page.</span></span>

![Exempelapp som körs lokalt](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

<span data-ttu-id="c3410-127">Tryck på **Ctrl+C** i terminalfönstret för att avsluta webbservern.</span><span class="sxs-lookup"><span data-stu-id="c3410-127">In your terminal window, press **Ctrl+C** to exit the web server.</span></span>

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Sida för tom webbapp](media/app-service-web-get-started-python/app-service-web-service-created.png)

<span data-ttu-id="c3410-129">Nu har du skapat en ny tom webbapp på Azure.</span><span class="sxs-lookup"><span data-stu-id="c3410-129">You’ve created an empty new web app in Azure.</span></span>

## <a name="configure-to-use-python"></a><span data-ttu-id="c3410-130">Konfiguration för användning av Python</span><span class="sxs-lookup"><span data-stu-id="c3410-130">Configure to use Python</span></span>

<span data-ttu-id="c3410-131">Använd kommandot [az webapp config set](/cli/azure/webapp/config#set) och konfigurera webbappen för att använda Python version `3.4`.</span><span class="sxs-lookup"><span data-stu-id="c3410-131">Use the [az webapp config set](/cli/azure/webapp/config#set) command to configure the web app to use Python version `3.4`.</span></span>

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


<span data-ttu-id="c3410-132">När du anger Python-versionen på det här sättet används en standardbehållare som tillhandahålls av plattformen.</span><span class="sxs-lookup"><span data-stu-id="c3410-132">Setting the Python version this way uses a default container provided by the platform.</span></span> <span data-ttu-id="c3410-133">Om du vill använda en egen behållare kan läsa CLI-referensen för kommandot [az webapp config container set](/cli/azure/webapp/config/container#set).</span><span class="sxs-lookup"><span data-stu-id="c3410-133">To use your own container, see the CLI reference for the [az webapp config container set](/cli/azure/webapp/config/container#set) command.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push to Azure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 18, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 4.31 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '44e74fe7dd'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling python deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'main.py'
remote: Copying file: 'README.md'
remote: Copying file: 'requirements.txt'
remote: Copying file: 'virtualenv_proxy.py'
remote: Copying file: 'web.2.7.config'
remote: Copying file: 'web.3.4.config'
remote: Detected requirements.txt.  You can skip Python specific steps with a .skipPythonDeployment file.
remote: Detecting Python runtime from site configuration
remote: Detected python-3.4
remote: Creating python-3.4 virtual environment.
remote: .................................
remote: Pip install requirements.
remote: Successfully installed Flask click itsdangerous Jinja2 Werkzeug MarkupSafe
remote: Cleaning up...
remote: .
remote: Overwriting web.config with web.3.4.config
remote:         1 file(s) copied.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-to-the-app"></a><span data-ttu-id="c3410-134">Bläddra till appen</span><span class="sxs-lookup"><span data-stu-id="c3410-134">Browse to the app</span></span>

<span data-ttu-id="c3410-135">Bläddra till den distribuerade appen via webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="c3410-135">Browse to the deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="c3410-136">Python-exempelkoden körs i en Azure App Service-webbapp.</span><span class="sxs-lookup"><span data-stu-id="c3410-136">The Python sample code is running in an Azure App Service web app.</span></span>

![Exempelapp som körs i Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="c3410-138">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="c3410-138">**Congratulations!**</span></span> <span data-ttu-id="c3410-139">Du har distribuerat din första Python-app till App Service.</span><span class="sxs-lookup"><span data-stu-id="c3410-139">You've deployed your first Python app to App Service.</span></span>

## <a name="update-and-redeploy-the-code"></a><span data-ttu-id="c3410-140">Uppdatera och distribuera om koden</span><span class="sxs-lookup"><span data-stu-id="c3410-140">Update and redeploy the code</span></span>

<span data-ttu-id="c3410-141">Öppna filen `main.py` i Python-appen med ett lokalt textredigeringsprogram och gör små ändringar i texten i strängen bredvid `return`-instruktionen:</span><span class="sxs-lookup"><span data-stu-id="c3410-141">Using a local text editor, open the `main.py` file in the Python app, and make a small change to the text next to the `return` statement:</span></span>

```python
return 'Hello, Azure!'
```

<span data-ttu-id="c3410-142">Spara ändringarna på Git och skicka sedan kodändringarna till Azure.</span><span class="sxs-lookup"><span data-stu-id="c3410-142">Commit your changes in Git, and then push the code changes to Azure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="c3410-143">När distributionen är klar går du tillbaka till webbläsarfönstret som öppnades när du skulle [söka efter appen](#browse-to-the-app) och klickar på knappen för att uppdatera sidan.</span><span class="sxs-lookup"><span data-stu-id="c3410-143">Once deployment has completed, switch back to the browser window that opened in the [Browse to the app](#browse-to-the-app) step, and refresh the page.</span></span>

![Uppdaterad exempelapp som körs i Azure](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="c3410-145">Hantera din nya Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="c3410-145">Manage your new Azure web app</span></span>

<span data-ttu-id="c3410-146">Gå till <a href="https://portal.azure.com" target="_blank">Azure Portal</a> för att hantera den webbapp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="c3410-146">Go to the <a href="https://portal.azure.com" target="_blank">Azure portal</a> to manage the web app you created.</span></span>

<span data-ttu-id="c3410-147">Klicka på **Apptjänster** i menyn till vänster och sedan på namnet på din Azure-webbapp.</span><span class="sxs-lookup"><span data-stu-id="c3410-147">From the left menu, click **App Services**, and then click the name of your Azure web app.</span></span>

![Navigera till webbappen på Azure Portal](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="c3410-149">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="c3410-149">You see your web app's Overview page.</span></span> <span data-ttu-id="c3410-150">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="c3410-150">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![App Service-blad på Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="c3410-152">Menyn till vänster innehåller olika sidor för att konfigurera appen.</span><span class="sxs-lookup"><span data-stu-id="c3410-152">The left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="c3410-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3410-153">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c3410-154">Python med PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="c3410-154">Python with PostgreSQL</span></span>](app-service-web-tutorial-docker-python-postgresql-app.md)
