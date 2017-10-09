---
title: aaaCreate en Python-webbapp i Azure | Microsoft Docs
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
ms.openlocfilehash: 42178d490d8aa8eaf93710667aad598794c62c8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-python-web-app-in-azure"></a><span data-ttu-id="8f9f5-103">Skapa en Python-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="8f9f5-103">Create a Python web app in Azure</span></span>

<span data-ttu-id="8f9f5-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="8f9f5-105">Denna Snabbstart går igenom hur toodevelop och distribuera en Python app tooAzure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-105">This quickstart walks through how toodevelop and deploy a Python app tooAzure Web Apps.</span></span> <span data-ttu-id="8f9f5-106">Du skapar hello webbapp med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), och du kan använda Git toodeploy Python-kod toohello exempelwebbapp.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli), and you use Git toodeploy sample Python code toohello web app.</span></span>

![Exempelapp som körs i Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="8f9f5-108">Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="8f9f5-109">När hello nödvändiga komponenter har installerats, tar cirka fem minuter toocomplete hello steg.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>
## <a name="prerequisites"></a><span data-ttu-id="8f9f5-110">Krav</span><span class="sxs-lookup"><span data-stu-id="8f9f5-110">Prerequisites</span></span>

<span data-ttu-id="8f9f5-111">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="8f9f5-111">toocomplete this tutorial:</span></span>

1. [<span data-ttu-id="8f9f5-112">Installera Git</span><span class="sxs-lookup"><span data-stu-id="8f9f5-112">Install Git</span></span>](https://git-scm.com/)
1. [<span data-ttu-id="8f9f5-113">Installera Python</span><span class="sxs-lookup"><span data-stu-id="8f9f5-113">Install Python</span></span>](https://www.python.org/downloads/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="8f9f5-114">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-114">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="8f9f5-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="8f9f5-116">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="8f9f5-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="download-hello-sample"></a><span data-ttu-id="8f9f5-117">Hämta hello-exempel</span><span class="sxs-lookup"><span data-stu-id="8f9f5-117">Download hello sample</span></span>

<span data-ttu-id="8f9f5-118">Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-118">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine.</span></span>

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

<span data-ttu-id="8f9f5-119">Du använder den här terminalfönster toorun alla hello-kommandon i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-119">You use this terminal window toorun all hello commands in this quickstart.</span></span>

<span data-ttu-id="8f9f5-120">Ändra toohello katalog som innehåller hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-120">Change toohello directory that contains hello sample code.</span></span>

```bash
cd Python-docs-hello-world
```

## <a name="run-hello-app-locally"></a><span data-ttu-id="8f9f5-121">Kör hello appen lokalt</span><span class="sxs-lookup"><span data-stu-id="8f9f5-121">Run hello app locally</span></span>

<span data-ttu-id="8f9f5-122">Installera hello krävs paket med hjälp av `pip`.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-122">Install hello required packages using `pip`.</span></span>

```bash
pip install -r requirements.txt
```

<span data-ttu-id="8f9f5-123">Kör programmet hello lokalt genom att öppna ett terminalfönster och hello `Python` kommandot toolaunch hello inbyggda Python webbservern.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-123">Run hello application locally by opening a terminal window and using hello `Python` command toolaunch hello built-in Python web server.</span></span>

```bash
python main.py
```

<span data-ttu-id="8f9f5-124">Öppna en webbläsare och gå toohello exempelapp på http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-124">Open a web browser, and navigate toohello sample app at http://localhost:5000.</span></span>

<span data-ttu-id="8f9f5-125">Du kan se hello **Hello World** meddelande från hello sample-appen visas i hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-125">You can see hello **Hello World** message from hello sample app displayed in hello page.</span></span>

![Exempelapp som körs lokalt](media/app-service-web-get-started-python/localhost-hello-world-in-browser.png)

<span data-ttu-id="8f9f5-127">Tryck på i ditt terminalfönster **Ctrl + C** tooexit hello-webbserver.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-127">In your terminal window, press **Ctrl+C** tooexit hello web server.</span></span>

[!INCLUDE [Log in tooAzure](../../includes/login-to-azure.md)] 

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)] 

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)] 

![Sida för tom webbapp](media/app-service-web-get-started-python/app-service-web-service-created.png)

<span data-ttu-id="8f9f5-129">Nu har du skapat en ny tom webbapp på Azure.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-129">You’ve created an empty new web app in Azure.</span></span>

## <a name="configure-toouse-python"></a><span data-ttu-id="8f9f5-130">Konfigurera toouse Python</span><span class="sxs-lookup"><span data-stu-id="8f9f5-130">Configure toouse Python</span></span>

<span data-ttu-id="8f9f5-131">Använd hello [az webapp konfigurationsuppsättning](/cli/azure/webapp/config#set) kommandot tooconfigure hello app toouse Python webbversionen `3.4`.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-131">Use hello [az webapp config set](/cli/azure/webapp/config#set) command tooconfigure hello web app toouse Python version `3.4`.</span></span>

```azurecli-interactive
az webapp config set --python-version 3.4 --name <app_name> --resource-group myResourceGroup
```


<span data-ttu-id="8f9f5-132">Ange hello Python-versionen sätt använder en standardbehållare som tillhandahålls av hello-plattformen.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-132">Setting hello Python version this way uses a default container provided by hello platform.</span></span> <span data-ttu-id="8f9f5-133">toouse egna behållaren finns hello CLI-referens för hello [az webapp konfigurationsuppsättning behållaren](/cli/azure/webapp/config/container#set) kommando.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-133">toouse your own container, see hello CLI reference for hello [az webapp config container set](/cli/azure/webapp/config/container#set) command.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 18, done.
Delta compression using up too4 threads.
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
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

## <a name="browse-toohello-app"></a><span data-ttu-id="8f9f5-134">Bläddra toohello app</span><span class="sxs-lookup"><span data-stu-id="8f9f5-134">Browse toohello app</span></span>

<span data-ttu-id="8f9f5-135">Bläddra toohello distribuerat program med hjälp av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-135">Browse toohello deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="8f9f5-136">hello Python exempelkod körs i en webbapp i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-136">hello Python sample code is running in an Azure App Service web app.</span></span>

![Exempelapp som körs i Azure](media/app-service-web-get-started-python/hello-world-in-browser.png)

<span data-ttu-id="8f9f5-138">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="8f9f5-138">**Congratulations!**</span></span> <span data-ttu-id="8f9f5-139">Du har distribuerat din första Python app tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-139">You've deployed your first Python app tooApp Service.</span></span>

## <a name="update-and-redeploy-hello-code"></a><span data-ttu-id="8f9f5-140">Uppdatera och distribuera hello kod</span><span class="sxs-lookup"><span data-stu-id="8f9f5-140">Update and redeploy hello code</span></span>

<span data-ttu-id="8f9f5-141">Med en lokal textredigerare öppna hello `main.py` filen i hello Python-appen och göra en mindre ändring toohello text nästa toohello `return` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="8f9f5-141">Using a local text editor, open hello `main.py` file in hello Python app, and make a small change toohello text next toohello `return` statement:</span></span>

```python
return 'Hello, Azure!'
```

<span data-ttu-id="8f9f5-142">Genomför ändringarna i Git och skicka sedan hello kod ändringar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-142">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="8f9f5-143">När distributionen är klar, kan du växla tillbaka toohello webbläsarfönster som öppnas i hello [Bläddra toohello app](#browse-to-the-app) steg och uppdatera hello sidan.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-143">Once deployment has completed, switch back toohello browser window that opened in hello [Browse toohello app](#browse-to-the-app) step, and refresh hello page.</span></span>

![Uppdaterad exempelapp som körs i Azure](media/app-service-web-get-started-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="8f9f5-145">Hantera din nya Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="8f9f5-145">Manage your new Azure web app</span></span>

<span data-ttu-id="8f9f5-146">Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toomanage hello webbprogram som du skapade.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-146">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="8f9f5-147">Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-147">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

<span data-ttu-id="8f9f5-149">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-149">You see your web app's Overview page.</span></span> <span data-ttu-id="8f9f5-150">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-150">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span> 

![App Service-blad på Azure Portal](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

<span data-ttu-id="8f9f5-152">hello vänstra menyn innehåller olika sidor för att konfigurera din app.</span><span class="sxs-lookup"><span data-stu-id="8f9f5-152">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="8f9f5-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f9f5-153">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="8f9f5-154">Python med PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="8f9f5-154">Python with PostgreSQL</span></span>](app-service-web-tutorial-docker-python-postgresql-app.md)
