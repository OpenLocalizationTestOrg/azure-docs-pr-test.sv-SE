---
title: aaaCreate en Ruby-App med Webbappar i Linux | Microsoft Docs
description: "Läs toocreate Ruby appar med Azure web wpp på Linux."
keywords: "Azure apptjänst, linux, oss, ruby"
services: app-service
documentationcenter: 
author: wesmc7777
manager: erikre
editor: 
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: wesmc;rachelap
ms.openlocfilehash: 99ce3b5ee16703a147787387bb02973defce8190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-ruby-app-with-web-apps-on-linux"></a><span data-ttu-id="d0435-104">Skapa en Ruby App med Webbappar i Linux</span><span class="sxs-lookup"><span data-stu-id="d0435-104">Create a Ruby App with Web Apps on Linux</span></span> 

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

<span data-ttu-id="d0435-105">Med [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) får du en mycket skalbar och automatiskt uppdaterad webbvärdtjänst.</span><span class="sxs-lookup"><span data-stu-id="d0435-105">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="d0435-106">Den här snabbstarten visar hur toocreate en grundläggande Ruby på spår programmet du sedan distribuera den tooAzure som ett webbprogram på Linux.</span><span class="sxs-lookup"><span data-stu-id="d0435-106">This quickstart shows you how toocreate a basic Ruby on Rails application you then deploy it tooAzure as a Web App on Linux.</span></span>

![Hello world](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

## <a name="prerequisites"></a><span data-ttu-id="d0435-108">Krav</span><span class="sxs-lookup"><span data-stu-id="d0435-108">Prerequisites</span></span>

* <span data-ttu-id="d0435-109">[Ruby 2.4.1 eller högre](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span><span class="sxs-lookup"><span data-stu-id="d0435-109">[Ruby 2.4.1 or higher](https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller).</span></span>
* <span data-ttu-id="d0435-110">[Git](https://git-scm.com/downloads).</span><span class="sxs-lookup"><span data-stu-id="d0435-110">[Git](https://git-scm.com/downloads).</span></span>
* <span data-ttu-id="d0435-111">En [aktiv Azure-prenumeration](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d0435-111">An [active Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a><span data-ttu-id="d0435-112">Hämta hello-exempel</span><span class="sxs-lookup"><span data-stu-id="d0435-112">Download hello sample</span></span>

<span data-ttu-id="d0435-113">Kör hello efter kommandot tooclone hello exempel app databasen tooyour lokala datorn i ett terminalfönster:</span><span class="sxs-lookup"><span data-stu-id="d0435-113">In a terminal window, run hello following command tooclone hello sample app repository tooyour local machine:</span></span>

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

## <a name="run-hello-application-locally"></a><span data-ttu-id="d0435-114">Kör hello programmet lokalt</span><span class="sxs-lookup"><span data-stu-id="d0435-114">Run hello application locally</span></span>

<span data-ttu-id="d0435-115">Köra hello spår server för hello programmet toowork.</span><span class="sxs-lookup"><span data-stu-id="d0435-115">Run hello rails server in order for hello application toowork.</span></span> <span data-ttu-id="d0435-116">Ändra toohello *hello world* katalog och hello `rails server` kommando startar hello server.</span><span class="sxs-lookup"><span data-stu-id="d0435-116">Change toohello *hello-world* directory, and hello `rails server` command starts hello server.</span></span>

```bash
cd hello-world\bin
rails server
```
    
<span data-ttu-id="d0435-117">Använd din webbläsare, navigera för`http://localhost:3000` tootest hello appen lokalt.</span><span class="sxs-lookup"><span data-stu-id="d0435-117">Using your web browser, navigate too`http://localhost:3000` tootest hello app locally.</span></span>  

![Hello world](./media/app-service-linux-ruby-get-started/hello-world.png)

## <a name="modify-app-toodisplay-welcome-message"></a><span data-ttu-id="d0435-119">Ändra app toodisplay välkomstmeddelande</span><span class="sxs-lookup"><span data-stu-id="d0435-119">Modify app toodisplay welcome message</span></span>

<span data-ttu-id="d0435-120">Ändra hello program så att det visar ett välkomstmeddelande.</span><span class="sxs-lookup"><span data-stu-id="d0435-120">Modify hello application so it displays a welcome message.</span></span> <span data-ttu-id="d0435-121">Ändra hello programmet domänkontrollant så att den returnerar hello-meddelande som HTML toohello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="d0435-121">Change hello application's controller so it returns hello message as HTML toohello browser.</span></span> 

<span data-ttu-id="d0435-122">Öppna *~/workspace/hello-world/app/controllers/application_controller.rb* för redigering.</span><span class="sxs-lookup"><span data-stu-id="d0435-122">Open *~/workspace/hello-world/app/controllers/application_controller.rb* for editing.</span></span> <span data-ttu-id="d0435-123">Ändra hello `ApplicationController` klassen toolook som hello följande kodexempel:</span><span class="sxs-lookup"><span data-stu-id="d0435-123">Modify hello `ApplicationController` class toolook like hello following code sample:</span></span>

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception 
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

<span data-ttu-id="d0435-124">Appen är nu konfigurerad.</span><span class="sxs-lookup"><span data-stu-id="d0435-124">Your app is now configured.</span></span> <span data-ttu-id="d0435-125">Använd din webbläsare, navigera för`http://localhost:3000` Landningssida för tooconfirm hello rot.</span><span class="sxs-lookup"><span data-stu-id="d0435-125">Using your web browser, navigate too`http://localhost:3000` tooconfirm hello root landing page.</span></span>

![Hello World konfigurerad](./media/app-service-linux-ruby-get-started/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-ruby-web-app-on-azure"></a><span data-ttu-id="d0435-127">Skapa en Ruby webbapp på Azure</span><span class="sxs-lookup"><span data-stu-id="d0435-127">Create a Ruby web app on Azure</span></span>

<span data-ttu-id="d0435-128">Använd hello [az programtjänstplan skapa](https://docs.microsoft.com/cli/azure/appservice/plan#create) kommandot toocreate en apptjänstplan för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d0435-128">Use hello [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) command toocreate an app service plan for your web app.</span></span> 
 
```azurecli-interactive
  az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

<span data-ttu-id="d0435-129">Därefter utfärda hello [az webapp skapa](https://docs.microsoft.com/cli/azure/webapp) kommandot toocreate hello med webbprogram som använder hello nyskapad service-plan.</span><span class="sxs-lookup"><span data-stu-id="d0435-129">Next, issue hello [az webapp create](https://docs.microsoft.com/cli/azure/webapp) command toocreate hello web app that uses hello newly created service plan.</span></span> <span data-ttu-id="d0435-130">Observera att hello-körning har angetts för`ruby|2.3`.</span><span class="sxs-lookup"><span data-stu-id="d0435-130">Notice that hello runtime is set too`ruby|2.3`.</span></span> <span data-ttu-id="d0435-131">Glöm inte tooreplace `<app name>` med ett unikt appnamn.</span><span class="sxs-lookup"><span data-stu-id="d0435-131">Don't forget tooreplace `<app name>` with a unique app name.</span></span>

```azurecli-interactive
  az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "ruby|2.3" --deployment-local-git
```

<span data-ttu-id="d0435-132">När hello webbprogram skapas en **översikt** är tillgängliga tooview.</span><span class="sxs-lookup"><span data-stu-id="d0435-132">Once hello web app is created, an **Overview** page is available tooview.</span></span> <span data-ttu-id="d0435-133">Navigera tooit.</span><span class="sxs-lookup"><span data-stu-id="d0435-133">Navigate tooit.</span></span> <span data-ttu-id="d0435-134">hello följande välkomstskärmen sida visas:</span><span class="sxs-lookup"><span data-stu-id="d0435-134">hello following splash page is displayed:</span></span>

![Välkomstskärmen sida](./media/app-service-linux-ruby-get-started/splash-page.png)


## <a name="deploy-your-application"></a><span data-ttu-id="d0435-136">Distribuera programmet</span><span class="sxs-lookup"><span data-stu-id="d0435-136">Deploy your application</span></span>

<span data-ttu-id="d0435-137">Använda Git toodeploy hello Ruby programmet tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d0435-137">Use Git toodeploy hello Ruby application tooAzure.</span></span> <span data-ttu-id="d0435-138">hello webbprogrammet har redan en Git-distribution som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="d0435-138">hello web app already has a Git deployment configured.</span></span> <span data-ttu-id="d0435-139">Du kan hämta hello distributionens URL genom att utfärda ett [az webapp distribution](https://docs.microsoft.com/cli/azure/webapp/deployment) kommando.</span><span class="sxs-lookup"><span data-stu-id="d0435-139">You can retrieve hello deployment URL by issuing an [az webapp deployment](https://docs.microsoft.com/cli/azure/webapp/deployment) command.</span></span>  

```bash
az webapp deployment source show --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="d0435-140">Observera att hello URL för Git har hello följande baserat på din webbprogrammets namn:</span><span class="sxs-lookup"><span data-stu-id="d0435-140">Notice that hello Git URL has hello following form based on your web app name:</span></span>

```bash
https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
```

[!INCLUDE [Clean-up section](../../includes/configure-deployment-user-no-h.md)]

<span data-ttu-id="d0435-141">Kör följande kommandon toodeploy hello lokalt program tooyour Azure-webbplatsen hello:</span><span class="sxs-lookup"><span data-stu-id="d0435-141">Run hello following commands toodeploy hello local application tooyour Azure website:</span></span>

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

<span data-ttu-id="d0435-142">Bekräfta att hello remote distributionsåtgärder rapporterar lyckades.</span><span class="sxs-lookup"><span data-stu-id="d0435-142">Confirm that hello remote deployment operations report success.</span></span> <span data-ttu-id="d0435-143">hello kommandon producerar utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="d0435-143">hello commands produce output similar toohello following text:</span></span>

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
toohttps://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

<span data-ttu-id="d0435-144">När hello distributionen är klar startar du om ditt webbprogram för hello distribution tootake effekt med hjälp av hello [az webapp omstart](https://docs.microsoft.com/cli/azure/webapp#restart) kommandot som visas här:</span><span class="sxs-lookup"><span data-stu-id="d0435-144">Once hello deployment has completed, restart your web app for hello deployment tootake effect by using hello [az webapp restart](https://docs.microsoft.com/cli/azure/webapp#restart) command, as shown here:</span></span>

```azurecli-interactive 
az webapp restart --name <app name> --resource-group myResourceGroup
```

<span data-ttu-id="d0435-145">Navigera tooyour plats och kontrollera hello resultat.</span><span class="sxs-lookup"><span data-stu-id="d0435-145">Navigate tooyour site and verify hello results.</span></span>

```bash
http://<your web app name>.azurewebsites.net
```
![uppdaterade för webbprogram](./media/app-service-linux-ruby-get-started/hello-world-updated.png)

> [!NOTE]
> <span data-ttu-id="d0435-147">Medan hello appen startas försöker toobrowse hello plats resulterar i en HTTP-statuskod `Error 503 Server unavailable`.</span><span class="sxs-lookup"><span data-stu-id="d0435-147">While hello app is restarting, attempting toobrowse hello site results in an HTTP status code `Error 503 Server unavailable`.</span></span> <span data-ttu-id="d0435-148">Det kan ta några minuter toofully omstart.</span><span class="sxs-lookup"><span data-stu-id="d0435-148">It may take a few minutes toofully restart.</span></span>
>

[!INCLUDE [Clean-up section](../../includes/cli-script-clean-up.md)]


## <a name="next-steps"></a><span data-ttu-id="d0435-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d0435-149">Next steps</span></span>

[<span data-ttu-id="d0435-150">Azure App Service Webbapp på Linux vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="d0435-150">Azure App Service Web App on Linux FAQ</span></span>](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)