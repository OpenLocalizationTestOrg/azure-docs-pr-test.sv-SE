---
title: aaaCreate en PHP-webbapp i Azure | Microsoft Docs
description: "Distribuera din första Hello World-app i PHP i Azure App Service Web Apps på bara några minuter."
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/21/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 8e1022889ca162f8f15ce7435cc9393cc6efef06
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-php-web-app-in-azure"></a><span data-ttu-id="36389-103">Skapa en PHP-webbapp i Azure</span><span class="sxs-lookup"><span data-stu-id="36389-103">Create a PHP web app in Azure</span></span>

<span data-ttu-id="36389-104">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="36389-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span>  <span data-ttu-id="36389-105">Den här snabbstartsguide visar hur toodeploy en PHP-app tooAzure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="36389-105">This quickstart tutorial shows how toodeploy a PHP app tooAzure Web Apps.</span></span> <span data-ttu-id="36389-106">Du skapar hello webbapp med hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) i molnet Shell, och du kan använda Git toodeploy PHP kod toohello exempelwebbapp.</span><span class="sxs-lookup"><span data-stu-id="36389-106">You create hello web app using hello [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) in Cloud Shell, and you use Git toodeploy sample PHP code toohello web app.</span></span>

<span data-ttu-id="36389-107">![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span><span class="sxs-lookup"><span data-stu-id="36389-107">![Sample app running in Azure]](media/app-service-web-get-started-php/hello-world-in-browser.png)</span></span>

<span data-ttu-id="36389-108">Du kan följa hello stegen nedan använder en Mac, Windows eller Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="36389-108">You can follow hello steps below using a Mac, Windows, or Linux machine.</span></span> <span data-ttu-id="36389-109">När hello nödvändiga komponenter har installerats, tar cirka fem minuter toocomplete hello steg.</span><span class="sxs-lookup"><span data-stu-id="36389-109">Once hello prerequisites are installed, it takes about five minutes toocomplete hello steps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36389-110">Krav</span><span class="sxs-lookup"><span data-stu-id="36389-110">Prerequisites</span></span>

<span data-ttu-id="36389-111">toocomplete denna Snabbstart:</span><span class="sxs-lookup"><span data-stu-id="36389-111">toocomplete this quickstart:</span></span>

* [<span data-ttu-id="36389-112">Installera Git</span><span class="sxs-lookup"><span data-stu-id="36389-112">Install Git</span></span>](https://git-scm.com/)
* [<span data-ttu-id="36389-113">Installera PHP</span><span class="sxs-lookup"><span data-stu-id="36389-113">Install PHP</span></span>](https://php.net)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample-locally"></a><span data-ttu-id="36389-114">Hämta hello exempel lokalt</span><span class="sxs-lookup"><span data-stu-id="36389-114">Download hello sample locally</span></span>

<span data-ttu-id="36389-115">Kör hello följande kommandon i ett terminalfönster.</span><span class="sxs-lookup"><span data-stu-id="36389-115">In a terminal window, run hello following commands.</span></span> <span data-ttu-id="36389-116">Detta klona hello exempel programmet tooyour lokala datorn och navigera toohello katalog som innehåller hello exempelkod.</span><span class="sxs-lookup"><span data-stu-id="36389-116">This will clone hello sample application tooyour local machine, and navigate toohello directory containing hello sample code.</span></span>

```bash
git clone https://github.com/Azure-Samples/php-docs-hello-world
cd php-docs-hello-world
```

## <a name="run-hello-app-locally"></a><span data-ttu-id="36389-117">Kör hello appen lokalt</span><span class="sxs-lookup"><span data-stu-id="36389-117">Run hello app locally</span></span>

<span data-ttu-id="36389-118">Kör programmet hello lokalt genom att öppna ett terminalfönster och hello `php` kommandot toolaunch hello inbyggda PHP webbservern.</span><span class="sxs-lookup"><span data-stu-id="36389-118">Run hello application locally by opening a terminal window and using hello `php` command toolaunch hello built-in PHP web server.</span></span>

```bash
php -S localhost:8080
```

<span data-ttu-id="36389-119">Öppna en webbläsare och gå toohello sample-appen på http://localhost: 8080.</span><span class="sxs-lookup"><span data-stu-id="36389-119">Open a web browser, and navigate toohello sample app at http://localhost:8080.</span></span>

<span data-ttu-id="36389-120">Du ser hello **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="36389-120">You see hello **Hello World!**</span></span> <span data-ttu-id="36389-121">meddelande från hello sample-appen visas i hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="36389-121">message from hello sample app displayed in hello page.</span></span>

![Exempelapp som körs lokalt](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

<span data-ttu-id="36389-123">Tryck på i ditt terminalfönster **Ctrl + C** tooexit hello-webbserver.</span><span class="sxs-lookup"><span data-stu-id="36389-123">In your terminal window, press **Ctrl+C** tooexit hello web server.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

[!INCLUDE [Create web app](../../includes/app-service-web-create-web-app.md)]

![Sida för tom webbapp](media/app-service-web-get-started-php/app-service-web-service-created.png)

<span data-ttu-id="36389-125">Nu har du skapat en ny tom webbapp på Azure.</span><span class="sxs-lookup"><span data-stu-id="36389-125">You’ve created an empty new web app in Azure.</span></span>

[!INCLUDE [Configure local git](../../includes/app-service-web-configure-local-git.md)] 

[!INCLUDE [Push tooAzure](../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 2, done.
Delta compression using up too4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 352 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '25f18051e9'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Copying file: 'index.php'
remote: Ignoring: .git
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<app_name>.scm.azurewebsites.net/<app_name>.git
   cc39b1e..25f1805  master -> master
```

## <a name="browse-toohello-app-locally"></a><span data-ttu-id="36389-126">Bläddra toohello appen lokalt</span><span class="sxs-lookup"><span data-stu-id="36389-126">Browse toohello app locally</span></span>

<span data-ttu-id="36389-127">Bläddra toohello distribuerat program med hjälp av webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="36389-127">Browse toohello deployed application using your web browser.</span></span>

```bash
http://<app_name>.azurewebsites.net
```

<span data-ttu-id="36389-128">hello PHP exempelkod körs i en webbapp i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="36389-128">hello PHP sample code is running in an Azure App Service web app.</span></span>

![Exempelapp som körs i Azure](media/app-service-web-get-started-php/hello-world-in-browser.png)

<span data-ttu-id="36389-130">**Grattis!**</span><span class="sxs-lookup"><span data-stu-id="36389-130">**Congratulations!**</span></span> <span data-ttu-id="36389-131">Du har distribuerat din första PHP app tooApp Service.</span><span class="sxs-lookup"><span data-stu-id="36389-131">You've deployed your first PHP app tooApp Service.</span></span>

## <a name="update-locally-and-redeploy-hello-code"></a><span data-ttu-id="36389-132">Uppdatera lokalt och omdistribuera hello kod</span><span class="sxs-lookup"><span data-stu-id="36389-132">Update locally and redeploy hello code</span></span>

<span data-ttu-id="36389-133">Med en lokal textredigerare öppna hello `index.php` filen i hello PHP-app och göra en mindre ändring toohello texten inom hello sträng bredvid för`echo`:</span><span class="sxs-lookup"><span data-stu-id="36389-133">Using a local text editor, open hello `index.php` file within hello PHP app, and make a small change toohello text within hello string next too`echo`:</span></span>

```php
echo "Hello Azure!";
```

<span data-ttu-id="36389-134">Genomför ändringarna i Git och skicka sedan hello kod ändringar tooAzure.</span><span class="sxs-lookup"><span data-stu-id="36389-134">Commit your changes in Git, and then push hello code changes tooAzure.</span></span>

```bash
git commit -am "updated output"
git push azure master
```

<span data-ttu-id="36389-135">När distributionen är klar, kan du växla tillbaka toohello webbläsarfönster som öppnas i hello **Bläddra toohello app** steg och uppdatera hello sidan.</span><span class="sxs-lookup"><span data-stu-id="36389-135">Once deployment has completed, switch back toohello browser window that opened in hello **Browse toohello app** step, and refresh hello page.</span></span>

![Uppdaterad exempelapp som körs i Azure](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a><span data-ttu-id="36389-137">Hantera din nya Azure-webbapp</span><span class="sxs-lookup"><span data-stu-id="36389-137">Manage your new Azure web app</span></span>

<span data-ttu-id="36389-138">Gå toohello <a href="https://portal.azure.com" target="_blank">Azure-portalen</a> toomanage hello webbprogram som du skapade.</span><span class="sxs-lookup"><span data-stu-id="36389-138">Go toohello <a href="https://portal.azure.com" target="_blank">Azure portal</a> toomanage hello web app you created.</span></span>

<span data-ttu-id="36389-139">Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på din Azure webbapp.</span><span class="sxs-lookup"><span data-stu-id="36389-139">From hello left menu, click **App Services**, and then click hello name of your Azure web app.</span></span>

![Portalen navigering tooAzure webbprogram](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

<span data-ttu-id="36389-141">Nu visas sidan Översikt för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="36389-141">You see your web app's Overview page.</span></span> <span data-ttu-id="36389-142">Här kan du utföra grundläggande hanteringsåtgärder som att bläddra, stoppa, starta, starta om och ta bort.</span><span class="sxs-lookup"><span data-stu-id="36389-142">Here, you can perform basic management tasks like browse, stop, start, restart, and delete.</span></span>

![App Service-blad på Azure Portal](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

<span data-ttu-id="36389-144">hello vänstra menyn innehåller olika sidor för att konfigurera din app.</span><span class="sxs-lookup"><span data-stu-id="36389-144">hello left menu provides different pages for configuring your app.</span></span> 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a><span data-ttu-id="36389-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="36389-145">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="36389-146">PHP med MySQL</span><span class="sxs-lookup"><span data-stu-id="36389-146">PHP with MySQL</span></span>](app-service-web-tutorial-php-mysql.md)
